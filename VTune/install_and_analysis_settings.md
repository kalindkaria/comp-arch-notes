# Installation and Settings for VTune analysis

- Installation: follow [this](https://www.intel.com/content/www/us/en/docs/vtune-profiler/installation-guide/2025-0/package-managers.html) link for no-gui installation process

- Run the below command to enable using all the VTune features:

    ```bash
    source /opt/intel/oneapi/vtune/latest/env/vars.sh
    ```

- Modify the below files to configure the system for profiling during various VTune analysis

    - Enable the profiling of the `ptrace` syscall using

        ```bash
        echo 0 > /proc/sys/kernel/yama/ptrace_scope
        ```

        - To make this change permanent, set the below parameter and reboot the machine

            ```bash
            # in file '/etc/sysctl.d/10-ptrace.conf'
            kernel.yama.ptrace_scope = 0
            ```
    - For the hardware event-based analysis, one of these actions are required

        1. Install Intel Sampling Drivers
        2. Configure driverless collection with Perf system-wide profiling

            - Make the below change

                ```bash
                echo 0 > /proc/sys/kernel/perf_event_paranoid
                ```

            - Usually, access to `/proc/kallsyms` file is limited. Consider making the below change to enable resolution of OS kernel and kernel module symbols

                ```bash
                echo 0 > /proc/sys/kernel/kptr_restrict
                ```

- The sampling drivers configured while installing the Intel VTune will be available to the `vtune` group. To add your user to this group, run the below command:

    ```bash
    sudo usermod -aG vtune $USER
    ```

- Run the `vtune-self-checker.sh`, a self-check script that helps verify if the product is installed correctly and troubleshoot issues, if any.
