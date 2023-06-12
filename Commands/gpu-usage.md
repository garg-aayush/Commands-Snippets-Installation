# Bash
Some useful and frequently used to monitor GPU usage

### nvidia-smi
```bash
# continuously monitor gpu usage
watch -n <time_in_sec> nvidia-smi
watch -n 1 nvidia-smi

# provide gpu in a particular format info every 2 seconds
nvidia-smi --query-gpu=name,memory.total,memory.free,memory.used --format=csv -l 2
```

