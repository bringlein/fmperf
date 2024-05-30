# Examples

## Structure (based on `example_benchmark.py`)

### Set the Kubernetes Configuration
Update the relevant sections under `if location == "local"` or `if location == "remote"` and set the appropriate value for `LOCATION` under `if __name__ == "__main__"`

### Define and initalize the Model Specification
There are three different ways to construct a model specification:
1. By calling the constructor of `TGISModelSpec` or `vLLMModelSpec` directly (as defined in [this file](https://github.ibm.com/ai-foundation/fmperf/blob/main/fmperf/ModelSpecs.py)).
2. By calling `TGISModelSpec.from_yaml` or `vLLMModelSpec.from_yaml` and passing a path to a YAML file defining the specification (see example [here](https://github.ibm.com/ai-foundation/fmperf/blob/main/examples/model_specifications_tgis_one.yml) for a `TGISModelSpec`).
3. By calling `TGISModelSpec.from_deployment` or `vLLMModelSpec.from_deployment` with path to where [BAM deployment configurations](https://github.ibm.com/ai-foundation/tgis-config) are checked out and the model ID. This will deploy a model with the same configuration that is used today in BAM.

### Define and initalize the Workload Specification
There are two different ways to construct a model specification:
1. By calling the constructor of `HomogeneousWorkloadSpec` or `HeterogeneousWorkloadSpec` directly (as defined in [this file](https://github.ibm.com/ai-foundation/fmperf/blob/main/fmperf/WorkloadSpecs.py)).
2. By calling `HomogeneousWorkloadSpec.from_yaml` or `HeterogeneousWorkloadSpec.from_yaml` and passing a path to a YAML file defining the specification 
(see an example [here](https://github.ibm.com/ai-foundation/fmperf/blob/main/examples/workload_specifications.yml) for a `HomogeneousWorkloadSpec`).

#### Homogeneous Workloads
Homogeneous workloads are workloads where all concurrent users of the inference server send the same request.
#### Heterogeneous Workloads
Heterogeneous workloads are workloads where all concurrent users of the inference server send different requests, 
with different number of input/output tokens, different temperature etc.
In this case, requests are generated using a statistical model that has been fitted to data from BAM logs. 
Therefore, we can consider this a somewhat realistic internal workload. 

### Execute `run_benchmark(...)` with the following parameters:
| Parameter     | Type                          | Definition                                                            |
|---------------|-------------------------------|-----------------------------------------------------------------------|
| cluster       | Object                        | Output of the Kubernetes Configuration                                |
| model_spec    | Object                        | Model specification                                                   |
| workload_spec | Object                        | Workload specification                                                |
| repetition    | Integer (e.g. 1)              | Number of times the experiment is to be repeated                      |
| number_users  | List of Integers (e.g [1, 2]) | Number of virtual concurrent users creating requests                  |
| duration      | String (e.g. "30s")           | Duration of experiment per each number of virtual concurrent users    |
| id            | Stringified UUID              | Unique identifier for the experiment                                  |