---
title: CPU Power
layout: page
---
# CPU Power

## Usage
```
cpupower [--cpu <cpus>] {frequency-info|frequency-set} <additional options>
```

Options for *frequency-info*:
```
--freq          Show Current Frequency
--governors     List Available governors
--policy        Display Current Policy
--
```

Options for *frequency-set*:
```
--freq                    Set Specific Frequency
--governor <governor>     Set Governor = {performance|userspace}
--max                     Set Max Frequency
--min                     Set Min Frequency
```

## Examples

Display Current CPU Power Configuration:
```
[root@host ~]# cpupower frequency-info
analyzing CPU 0:
   driver: powernv-cpufreq
   CPUs which run at the same hardware frequency: 0 1 2 3 4 5 6 7
   CPUs which need to have their frequency coordinated by software: 0
   hardware limits: 2.06 GHz - 4.02 GHz
   available frequency steps: 4.02 GHz, 3.99 GHz, 3.96 GHz, 3.92 GHz, 3.89 GHz, 3.86 GHz, 3.82 GHz, 3.79 GHz, 3.76 GHz, 3.72 GHz, 3.69 GHz, 3.66 GHz, 3.62 GHz, 3.59 GHz, 3.56 GHz, 3.52 GHz, 3.49 GHz, 3.46 GHz, 3.42 GHz, 3.39 GHz, 3.36 GHz, 3.33 GHz, 3.29 GHz, 3.26 GHz, 3.23 GHz, 3.19 GHz, 3.16 GHz, 3.13 GHz, 3.09 GHz, 3.06 GHz, 3.03 GHz, 2.99 GHz, 2.96 GHz, 2.93 GHz, 2.89 GHz, 2.86 GHz, 2.83 GHz, 2.79 GHz, 2.76 GHz, 2.73 GHz, 2.69 GHz, 2.66 GHz, 2.63 GHz, 2.59 GHz, 2.56 GHz, 2.53 GHz, 2.49 GHz, 2.46 GHz, 2.43 GHz, 2.39 GHz, 2.36 GHz, 2.33 GHz, 2.29 GHz, 2.26 GHz, 2.23 GHz, 2.19 GHz, 2.16 GHz, 2.13 GHz, 2.09 GHz, 2.06 GHz
   available cpufreq governors: conservative, userspace, powersave, ondemand, performance
   current policy: frequency should be within 2.06 GHz and 4.02 GHz.
                   The governor "performance" may decide which speed to use
                   within this range.
   current CPU frequency is 4.02 GHz (asserted by call to hardware).
```

Set Governor:
```
cpupower frequency-set --governor userspace
```

Set CPU Min/Max Frequency:
```
cpupower frequency-set --max 2.89GHz --min 2.89GHz
```

Set CPU Frequency:
```
cpupower frequency-set --freq 2.89GHz"
```
