✅ 0. The goal of phase 2b is to perform benchmarking/scalability tests of sample three-tier lakehouse solution.

✅ 1. In main.tf, change machine_type at:

```
module "dataproc" {
  depends_on   = [module.vpc]
  source       = "github.com/bdg-tbd/tbd-workshop-1.git?ref=v1.0.36/modules/dataproc"
  project_name = var.project_name
  region       = var.region
  subnet       = module.vpc.subnets[local.notebook_subnet_id].id
  machine_type = "e2-standard-2"
}
```

and subsititute "e2-standard-2" with "e2-standard-4".

✅ 2. If needed request to increase cpu quotas (e.g. to 30 CPUs): 
https://console.cloud.google.com/apis/api/compute.googleapis.com/quotas?project=tbd-2023z-9918

✅ 3. Using tbd-tpc-di notebook perform dbt run with different number of executors, i.e., 1, 2, and 5, by changing:
```
 "spark.executor.instances": "2"
```

in profiles.yml.

✅ 4. In the notebook, collect console output from dbt run, then parse it and retrieve total execution time and execution times of processing each model. Save the results from each number of executors. 

✅ 5. Analyze the performance and scalability of execution times of each model. Visualize and discucss the final results.

![img.png](photos/execution_times_plot.jpg)
![img.png](photos/execution_times_table.jpg)

| Number of Executors | Total Execution Time |
|---------------------|----------------------|
| 1                   | 771.34s              |
| 2                   | 586.49s              |
| 5                   | 439.58s              |


Overall, increasing the number of executors tends to decrease the duration of running the code for most table models.
For some table models, the duration decreases significantly with more executors, indicating good scalability.
However, the extent of improvement varies across different table models.
Some table models show consistent durations across different numbers of executors, suggesting that they may not benefit much from additional executors.
It's important to note that the performance improvement by increasing executors depends on various factors including the nature of the computation, data distribution, and cluster configuration.
   
