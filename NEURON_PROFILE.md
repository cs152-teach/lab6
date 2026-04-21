# Neuron Profile User Guide
Neuron Profile is an AWS tool to allow users to view the performance metrics of programs run on Neuron Devices like Trainium. Read the [Neuron Profile User Guide](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/tools/neuron-sys-tools/neuron-profile-user-guide.html) for more detailed information.

## Using neuron-profile

### Generating NEFF and NTFF Files
In order to view the execution profile of a NKI kernel you need the NEFF and NTFF files. The NEFF file is the compiled instructions from the python kernel that are actually executed on the NeuronCore. The NTFF file is used to record various metrics of the memory and compute engines while profiling the execution of the kernel.

You can generate the profile files by adding the `--profile` flag when running the `tester.py` script, which the NTFF/NEFF files in the `nki_conv2d/profiles` folder.

### Viewing Profiles
Now, use `neuron-profile` to view the profile GUI.
```bash
neuron-profile view -n <file name>.neff -s <file name>.ntff
```
This will take a while to load, but once it does, it will output a localhost link you can click to view the GUI.

Make sure you have port forwarding enabled. You can run this command in a separate terminal on your **local machine** to enable port forwarding:
```bash
ssh trn1_cs152 -L 3001:localhost:3001 -L 8086:localhost:8086
```

### Interpreting the Neuron Profile GUI
It is suggested to read the [Neuron Profile User Guide](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/tools/neuron-sys-tools/neuron-profile-user-guide.html) to better understand how to interact with the Neuron Profile GUI and interpret the results. Specifically, the [Understanding a Neuron profile](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/tools/profiler/neuron-profile-user-guide.html#understanding-a-neuron-profile) section will be useful.

Here are some steps you can take to interpret the profile:
- We suggest hiding the DMA profiling rows initially to first understand the utilization of the compute engines. Select 'View Settings', and toggle 'Show dma' off, then click the orange 'Save' button. You can re-enable the dma rows later if you want to look into the data movement and if data is being spilled from the SBUF.
- Zoom in on a short time period by selecting a narrow horizontal range on the timeline. Click at the top of the profile and drag to the right to select the narrow time range. This will allow you to better visualize the utilization of the compute engines
- You can also view the summary statistics by clicking on the "Summary." Look at "overall_stats > hardware_flops", "tensor_engine > tensor_engine_active_time_percent", and other statistics to get a better understand of how your kernel is using the hardware.

Analyzing Instructions:
- If you click on a single horizontal bar for the engine instructions, and scroll down to the "Details" section, you will see a lines saying something like: "Bir debug info source location: /home/ubuntu/lab6/nki_conv2d/conv2d.py:90." The "90" corresponds to the NKI source code line that this instruction is associated with. You can use this information to map the profile instructions with your NKI code, which may help you debug.
- When you click on a instruction/bar, other bars will be highlighted in light blue. These indicate the dependencies between this instruction and other instructions.
