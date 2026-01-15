# CodeGenRCA

![Python Version](https://img.shields.io/badge/Python-3776AB?&logo=python&logoColor=white-blue&label=3.12)&ensp;

We propose CodeGenRCA, a generalizable RCA solution that performs collaborative analysis on multi-modal observability data. CodeGenRCA eliminates the need for prebuilt tools or historical incident reports. It uses an LLM-based multi-agent system (MAS) and the two-stage tool generation with feedback-based refinement to better interact with observability data and perform collaborative analysis. 

Experiments on public datasets  from three real-world systems  containing 335  incidents show that CodeGenRCA achieves state-of-the-art RCA performance, with a score rate of 0.80.

## ✨ Features
- LLM-based multi-agent system (MAS)
- Two-stage tool generation with feedback-based refinement
- Better interact with observability data and perform collaborative analysis. 

## 💡Prerequisites
CodeGenRCA requires **Python >= 3.12**. 
```
# create conda env
conda create -n codegenrca python==3.12
conda activate codegenrca
pip install -r requirements.txt
```
### docker
To ensure environmental safety, we run the generated code in Docker. Therefore, please make sure that Docker is installed on your machine before running the code.
Run the following command to check if the Docker daemon is running:
```
docker version
```
If the terminal outputs `Cannot connect to the Docker daemon at xxx Is the docker daemon running?`, please check the running status of Docker again.

For Linux machines, please refer to: [Linux | Docker Docs](https://docs.docker.com/desktop/setup/install/linux/)
For Mac machines, in addition to Docker Desktop, you can also use Orbstack as an alternative:
```
brew install orbstack
```
### dataset
In addition to the environment, we use OpenRCA as the dataset. You can download the data from [Google Drive](https://drive.google.com/drive/folders/1wGiEnu4OkWrjPxfx5ZTROnU37-5UDoPM) and then place it in the `coding/dataset` directory under the coding file set.
```
├── coding
│   └── dataset
│       └── Bank
│           └── telemetry
│               ├── 2021_03_04
│               │   ├── log
│               │   │   └── log_service.csv
│               │   ├── metric
│               │   │   ├── metric_app.csv
│               │   │   └── metric_container.csv
│               │   └── trace
│               │       └── trace_span.csv
│               └── 2021_03_05
│                   ├── log
│                   │   └── log_service.csv
│                   ├── metric
│                   │   ├── metric_app.csv
│                   │   └── metric_container.csv
│                   └── trace
│                       └── trace_span.csv
...
...

```
## 🛠️ How to Run
First, you need to add your api_key in `agent.py`.
```python
reason_model_client =  OpenAIChatCompletionClient(
        model="",
        base_url="",
        api_key="",
        model_info={
        "vision": True,
        "function_calling": True,
        "json_output": False,
        "family": "unknown",
        },
        temperature=0,
)

model_client =  OpenAIChatCompletionClient(
        model="",
        base_url="",
        api_key="",
        model_info={
        "vision": True,
        "function_calling": True,
        "json_output": False,
        "family": "unknown",
        },
        temperature=0,
)
```
Then,  you can run CodeGenRCA to perform RCA on a given query by running:
```bash
python codegenrca.py --query "On March 4, 2021, between 18:00 and 18:30, there was a single failure observed in the system. The exact component that caused this failure is unknown, and the reason behind the failure is also undetermined. Your task is to identify the root cause component and the root cause reason for this failure."
```

## 📊 How to Evaluate
We evaluate CodeGenRCA on three real-world systems: Bank, Market, and Telecom. 
You can reproduce the evaluation results by running:
```bash
python -m eval.evaluate \
    -p \
      ./archive/codegenrca-eval-bank.csv \
      ./archive/codegenrca-eval-telecom.csv \
      ./archive/codegenrca-eval-market1.csv \
      ./archive/codegenrca-eval-market2.csv \
    -q \
      ./query/bank_query.csv \
      ./query/telecom_query.csv \
      ./query/market1_query.csv \
      ./query/market2_query.csv \
    -r \
      ./report.csv
```
