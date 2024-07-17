# nextflow-issues

- how to optimize sequential workflow
  
process with `.out.collect()` not rerunning if the first process has been run, e.g
```
promle( data )
integration_plot("${params.project_folder}/${params.output_mle}/", promle.out.collect(), matrices)
```
- work around using the same process twice within a workflow
for example, this will not work because `promle` cannot call twice
```
if ( 'output_mle' in params.keySet() ) {
  data = channel.fromPath( "${params.project_folder}/${params.output_mle}/*mle.sh" )
  data = data.filter{ ! file( "$it".replace(".mle.sh", ".sgrna_summary.txt") ).exists() }
  promle( data )
      
  if ( 'mle_matrices' in params.keySet() ) {
    data = channel.fromPath( "${params.project_folder}/${params.output_mle}/mle_matrix/*mle.sh" )
    data = data.filter{ ! file( "$it".replace(".mle.sh", ".sgrna_summary.txt") ).exists() }
    promle( data )
    matrices="${params.mle_matrices}"
    integration_plot("${params.project_folder}/${params.output_mle}/mle_matrix/", promle.out.collect(), matrices)
  }
```
