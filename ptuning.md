ChatGLM2-6B官方Fine-tuning用的是P-tuning V2。在使用过程中遇到好多坑

## 坑王 nohup

按照官方文档描述，训练模型时会最终执行`bash train.sh`，训练时间很长，需要后台运行，所以使用`nohup`命令

```bash
nohup bash train.sh > log2.txt &
```

因为这个坑王，耽误我一周多时间。

每次运行，训练到一定程度，就会遇到以下错误。

```bash

 29%|██▊       | 1428/5000 [6:15:51<15:39:45, 15.79s/it]WARNING:torch.distributed.elastic.agent.server.api:Received 1 death signal, shutting down workers
WARNING:torch.distributed.elastic.multiprocessing.api:Sending process 44927 closing signal SIGHUP
Traceback (most recent call last):
  File "/usr/local/bin/torchrun", line 8, in <module>
    sys.exit(main())
  File "/usr/local/lib/python3.8/dist-packages/torch/distributed/elastic/multiprocessing/errors/__init__.py", line 346, in wrapper
    return f(*args, **kwargs)
  File "/usr/local/lib/python3.8/dist-packages/torch/distributed/run.py", line 794, in main
    run(args)
  File "/usr/local/lib/python3.8/dist-packages/torch/distributed/run.py", line 785, in run
    elastic_launch(
  File "/usr/local/lib/python3.8/dist-packages/torch/distributed/launcher/api.py", line 134, in __call__
    return launch_agent(self._config, self._entrypoint, list(args))
  File "/usr/local/lib/python3.8/dist-packages/torch/distributed/launcher/api.py", line 241, in launch_agent
    result = agent.run()
  File "/usr/local/lib/python3.8/dist-packages/torch/distributed/elastic/metrics/api.py", line 129, in wrapper
    result = f(*args, **kwargs)
  File "/usr/local/lib/python3.8/dist-packages/torch/distributed/elastic/agent/server/api.py", line 723, in run
    result = self._invoke_run(role)
  File "/usr/local/lib/python3.8/dist-packages/torch/distributed/elastic/agent/server/api.py", line 864, in _invoke_run
    time.sleep(monitor_interval)
  File "/usr/local/lib/python3.8/dist-packages/torch/distributed/elastic/multiprocessing/api.py", line 62, in _terminate_process_handler
    raise SignalException(f"Process {os.getpid()} got signal: {sigval}", sigval=sigval)
torch.distributed.elastic.multiprocessing.api.SignalException: Process 44915 got signal: 1
```

开始只是以为环境问题还是其他问题。每次就重新跑。每次跑每次都会遇到这个问题。多台机器跑，每台机器还是会遇到这个问题。

草，是一种绿色植物。

海草，海草，海草。海草，海草，海草。海草，海草，海草。海草，海草，海草。

搜索 之后找到[原因](https://discuss.pytorch.org/t/ddp-error-torch-distributed-elastic-agent-server-api-received-1-death-signal-shutting-down-workers/135720)

> 原文说到：
> 
> So I was able to repro this with nohup. Currently torch.distributed.launch, torchrun, torch.distributed.run will all not work with nohup since we register our own termination handler for SIGHUP, which overrides the ignore handler by nohup. If you need to close the terminal but keep the process running, then you can use tmux or screen.

用`tmux`或者`screen`命令代替`nohup`。