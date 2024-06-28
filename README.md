# nextflow-issues

- how to optimize sequential workflow
  
process with `.out.collect()` not rerunning if the first process has been run, e.g
```
promle( data )
integration_plot("${params.project_folder}/${params.output_mle}/", promle.out.collect(), matrices)
```
