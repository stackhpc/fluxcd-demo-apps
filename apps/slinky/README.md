# Slinky setup

Based on:
https://github.com/SlinkyProject/slurm-operator/blob/main/docs/user/quickstart.md

# Testing Slinky Slurm

To test Slurm functionality, connect to the controller to use Slurm client
commands:

```sh
kubectl --namespace=slurm exec -it statefulsets/slurm-controller -- bash --login
```

On the controller pod (e.g. host `slurm@slurm-controller-0`), run the following
commands to quickly test Slurm is functioning:

```sh
sinfo
srun hostname
sbatch --wrap="sleep 60"
squeue
```

See [Slurm Commands][slurm-commands] for more details on how to interact with
Slurm.

<!-- Links -->

[slurm-commands]: https://slurm.schedmd.com/quickstart.html#commands