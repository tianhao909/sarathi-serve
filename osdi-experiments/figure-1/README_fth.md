# vLLM Tail Latency (Figure 1)

## Experiment Setup

Both experiments use Yi-34B with 2 A100 80GB PCIe GPUs that are connected with NVLINK in a pairwise manner for 2-way tensor parallelism. It was run in a single node (Azure NC96ads v4 VMs) containing 4 such GPUs. Before starting the job, start ray on the cluster and onboard all GPUs using

vLLM 尾部延迟（图1）
实验设置
两个实验都使用搭载2个A100 80GB PCIe GPU的Yi-34B，这些GPU通过NVLINK以成对方式连接，实现2路张量并行。实验在单个节点（Azure NC96ads v4虚拟机）上运行，该节点包含4个这样的GPU。在开始作业之前，使用以下命令在集群上启动Ray并使用所有GPU：

```sh
ray stop
ray start --head
```

## Generation Stalls (Figure 1a)

### Experiment Design

We show that the `vLLM` scheduler has considerable gaps where no decode tokens are output, while the `sarathi` scheduler has no such gaps and continues to produce decode tokens unfettered. We show this by running the `vLLM` and `sarathi` schedulers on the same model (Yi-34B TP2) and trace (`arxiv summarization`) and comparing the decode completion times.
生成停滞（图1a）
实验设计
我们展示vLLM调度器在没有解码令牌输出时存在相当大的间隙，而sarathi调度器没有这样的间隙，并且能够不受阻碍地持续产生解码令牌。我们通过在相同的模型（Yi-34B TP2）和追踪（arxiv摘要）上运行vLLM和sarathi调度器，并比较解码完成时间来展示这一点。

### Running the experiment

After setting up the repository on the appropriate hardware platform as described above, run the following commands from the root of the repository. This script has two runs each taking roughly 10 minutes to complete.
运行实验
在如上所述的适当硬件平台上设置好仓库后，从仓库根目录运行以下命令。这个脚本有两个运行，每个大约需要10分钟才能完成。

```sh
bash ./osdi-experiments/figure-1/generation_stalls.sh
```

This `.sh` file can also be generated using the `generate_runs()` function in the notebook `generation_stalls.ipynb`.

### Interpreting the results

Run the `plot()` function in  `generation_stalls.ipynb` to plot the decode completion time series. The `vllm` series will have steps while `sarathi` one will be smooth. You can also analyze the raw CSVs in path `osdi-experiments/figure-1/benchmark_output/<timestamp>/replica_0/plots/decode_completion_time_series.csv`.

## High tail latency (Figure 1b)

### Experiment Design

We show that when hit with the same workload, the `vLLM` scheduler has a higher tail latency compared to the `sarathi` scheduler. We show this by running the `vLLM` and `sarathi` schedulers on the same model (Yi-34B TP2) and trace (`arxiv summarization`) and comparing the tail latencies for three QPS values.

### Running the experiment

After setting up the repository on the appropriate hardware platform as described above, run the following commands from the root of the repository. This script has 6 runs and will take roughly 30 minutes to complete.

```sh
bash ./osdi-experiments/figure-1/high_tail_latency.sh
```

This `.sh` file can also be generated using the `generate_runs()` function in the notebook `high_tail_latency.ipynb`.

### Interpreting the results

Run the `plot()` function in  `high_tail_latency.ipynb` to plot the decode completion time series. The `vllm` series will have steps while `sarathi` one will be smooth. You can also analyze the raw CSVs in path `high_tail_latency_output/<timestamp>/replica_0/plots/decode_token_execution_plus_preemption_time.csv`.







sh
bash ./osdi-experiments/figure-1/generation_stalls.sh
这个.sh文件也可以使用笔记本generation_stalls.ipynb中的generate_runs()函数生成。

解释结果
在generation_stalls.ipynb中运行plot()函数来绘制解码完成时间序列。vllm系列将有步骤，而sarathi系列将是平滑的。您还可以分析路径osdi-experiments/figure-1/benchmark_output/<timestamp>/replica_0/plots/decode_completion_time_series.csv中的原始CSV文件。

高尾部延迟（图1b）
实验设计
我们展示在相同的工作负载下，vLLM调度器的尾部延迟比sarathi调度器更高。我们通过在相同的模型（Yi-34B TP2）和追踪（arxiv摘要）上运行vLLM和sarathi调度器，并比较三个QPS值的尾部延迟来展示这一点。

运行实验
在如上所述的适当硬件平台上设置好仓库后，从仓库根目录运行以下命令。这个脚本有6个运行，将需要大约30分钟才能完成。

sh
bash ./osdi-experiments/figure-1/high_tail_latency.sh
这个.sh文件也可以使用笔记本high_tail_latency.ipynb中的generate_runs()函数生成。

解释结果
在high_tail_latency.ipynb中运行plot()函数来绘制解码完成时间序列。vllm系列将有步骤，而sarathi系列将是平滑的。您还可以分析路径high_tail_latency_output/<timestamp>/replica_0/plots/decode_token_execution_plus_preemption_time.csv中的原始CSV文件。