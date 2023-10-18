# `snpe-net-run`

```
snpe-net-run --help
Command Line Options:
  [ -h, --help ]        Displays this help message.
  [ --version ]         Displays version information.
  --container=<val>     Path to the DL container containing the network.
  --input_list=<val>    Path to a file listing the inputs for the network.
                        Optionally the file can have "#" starting line to specify the layer names or "%" to sepecify the output tensor
                        names for which output tensor files are to be produced. For more details about the input_list file format,
                        please refer to SDK html documentation (docs/general/tools.html#snpe-net-run input_list argument)
  [ --use_gpu ]         Use the GPU runtime.
  [ --use_dsp ]         Use the DSP fixed point runtime.
  [ --use_aip ]         Use the AIP fixed point runtime.
  [ --debug ]           Specifies that output from all layers of the network
                        will be saved.
  [ --output_dir=<val> ]
                        The directory to save output to. Defaults to ./output
  [ --storage_dir=<val> ]
                        The directory to store metadata files
  [ --encoding_type=<val> ]
                        Specifies the encoding type of input file. Valid settings are "nv21".
                        Cannot be combined with --userbuffer*.
  [ --use_native_input_files ]
                        Specifies to consume the input file(s) in their native data type(s).
                        Must be used with --userbuffer_xxx.
  [ --use_native_output_files ]
                        Specifies to write the output file(s) in their native data type(s).
                        Must be used with --userbuffer_xxx.
  [ --userbuffer_auto ]
                        Specifies to use userbuffer for input and output, with auto detection of types enabled.
                        Must be used with user specified buffer. Cannot be combined with --encoding_type.
  [ --userbuffer_float ]
                        Specifies to use userbuffer for inference, and the input type is float.
                        Cannot be combined with --encoding_type.
  [ --userbuffer_floatN=<val> ]
                        Specifies to use userbuffer for inference, and the input type is float 16 or float 32.
                        Cannot be combined with --encoding_type.
  [ --userbuffer_tf8 ]  Specifies to use userbuffer for inference, and the input type is tf8exact0.
                        Cannot be combined with --encoding_type.
  [ --userbuffer_tfN=<val> ]
                        Overrides the userbuffer output used for inference, and the output type is tf8exact0 or tf16exact0.
                        Must be used with user specified buffer.
  [ --userbuffer_float_output ]
                        Overrides the userbuffer output used for inference, and the output type is float. Must be used with user
                        specified buffer.
  [ --userbuffer_floatN_output=<val> ]
                        Overrides the userbuffer output used for inference, and the output type is float 16 or float 32. Must be used with user
                        specified buffer.
  [ --userbuffer_tfN_output=<val> ]
                        Overrides the userbuffer output used for inference, and the output type is tf8exact0 or tf16exact0.
                        Must be used with user specified buffer.
  [ --userbuffer_tf8_output ]
                        Overrides the userbuffer output used for inference, and the output type is tf8exact0.
  [ --userbuffer_uintN_output=<val> ]
                        Overrides the userbuffer output used for inference, and the output type is Uint N. Must be used with user
                        specified buffer.
  [ --userbuffer_memorymapped ]
                        Specifies to use ION (zero-copy) user buffer. Must be used with --userbuffer_float or 
                        --userbuffer_tf8 or userbuffer_tfN or userbuffer_auto etc. Cannot be combined with --encoding_type.
                        Note: Passing this option will turn all input and output userbuffers into memory mapped buffer
  [ --static_min_max ]  Specifies to use quantization parameters from the model instead of
                        input specific quantization. Used in conjunction with --userbuffer_tf8.
  [ --resizable_dim=<val> ]
                        Specifies the maximum number that resizable dimensions can grow into.
                        Used as a hint to create UserBuffers for models with dynamic sized outputs. Should be a
                        positive integer and is not applicable when using ITensor.
  [ --userbuffer_glbuffer ]
                        [EXPERIMENTAL]  Specifies to use userbuffer for inference, and the input source is OpenGL buffer.
                        Cannot be combined with --encoding_type.
                        GL buffer mode is only supported on Android OS.
  [ --data_type_map=<val> ]
                        Sets data type of IO buffers during prepare.
                        Arguments should be provided in the following format:
                        --data_type_map buffer_name1=buffer_name1_data_type --data_type_map buffer_name2=buffer_name2_data_type
                        Data Type can have the following values: float16, float32, fixedPoint8, fixedPoint16, int8, int32, uint8, uint32
                        Note: Must use this option with --tensor_mode.
  [ --tensor_mode=<val> ]
                        Sets type of tensor to use.
                        Arguments should be provided in the following format:
                        --tensor_mode iTensor 
                        Data Type can have the following values: userBuffer, iTensor
  [ --perf_profile=<val> ]
                        Specifies perf profile to set. Valid settings are "low_balanced" , "balanced" , "default",
                        "high_performance" ,"sustained_high_performance", "burst", "low_power_saver", "power_saver",
                        "high_power_saver", "extreme_power_saver", and "system_settings".
  [ --profiling_level=<val> ]
                        Specifies the profiling level.  Valid settings are "off", "basic", "moderate", "detailed" and "linting".
                        Default is detailed.
  [ --enable_cpu_fallback ]
                        Enables cpu fallback functionality. Defaults to disable mode.
  [ --input_name=<val> ]
                        Specifies the name of input for which dimensions are specified.
  [ --input_dimensions=<val> ]
                        Specifies new dimensions for input whose name is specified in input_name. e.g. "1,224,224,3".
                        For multiple inputs, specify --input_name and --input_dimensions multiple times.
  [ --gpu_mode=<val> ]  Specifies gpu operation mode. Valid settings are "default", "float16".
                        default = float32 math and float16 storage (equiv. use_gpu arg).
                        float16 = float16 math and float16 storage.
  [ --enable_init_cache ]
                        Enable init caching mode to accelerate the network building process. Defaults to disable.
  [ --platform_options=<val> ]
                        Specifies value to pass as platform options.
  [ --priority_hint=<val> ]
                        Specifies hint for priority level.  Valid settings are "low", "normal", "normal_high", "high". Defaults to normal.
                        Note: "normal_high" is only available on DSP.
  [ --inferences_per_duration=<val> ]
                        Specifies the number of inferences in specific duration (in seconds). e.g. "10,20".
  [ --runtime_order=<val> ]
                        Specifies the order of precedence for runtime e.g  cpu_float32, dsp_fixed8_tf etc
                        Valid values are:- 
                        cpu_float32 (Snapdragon CPU)       = Data & Math: float 32bit
                        gpu_float32_16_hybrid (Adreno GPU) = Data: float 16bit Math: float 32bit
                        dsp_fixed8_tf (Hexagon DSP)        = Data & Math: 8bit fixed point Tensorflow style format
                        gpu_float16 (Adreno GPU)           = Data: float 16bit Math: float 16bit
  [ --set_output_tensors=<val> ]
                        Specifies a comma separated list of tensors to be output after execution.
  [ --set_unconsumed_as_output ]
                        Sets all unconsumed tensors as outputs.
                        aip_fixed8_tf (Snapdragon HTA+HVX) = Data & Math: 8bit fixed point Tensorflow style format
                        cpu (Snapdragon CPU)               = Same as cpu_float32
                        gpu (Adreno GPU)                   = Same as gpu_float32_16_hybrid
                        dsp (Hexagon DSP)                  = Same as dsp_fixed8_tf 
                        aip (Snapdragon HTA+HVX)           = Same as aip_fixed8_tf
  [ --udo_package_path=<val> ]
                        Path to the registration library for UDO package(s).
                        Optionally, user can provide multiple packages as a comma-separated list.
  [ --duration=<val> ]  Specified the duration of the run in seconds. Loops over the input_list until this amount of time has transpired.
  [ --enable_cpu_fxp ]  Enable the fixed point execution on CPU runtime
  [ --dbglogs ]         
  [ --timeout=<val> ]   Execution terminated when exceeding time limit (in microseconds). Only valid for HTP(dsp v68+) runtime.
  [ --userlogs=<val> ]  Specifies the user level logging as level,<optional logPath>.
                        Valid values are: "warn", "verbose", "info", "error", "fatal"
```

# `setup_inceptionv3.py`

```
python3 /opt/qcom/aistack/snpe/2.14.2.230905/examples/Models/inception_v3/scripts/setup_inceptionv3.py --help
usage: /opt/qcom/aistack/snpe/2.14.2.230905/examples/Models/inception_v3/scripts/setup_inceptionv3.py [-h] -a ASSETS_DIR [-d] [-r {cpu,gpu,dsp,aip,all}] [-u] [-l [HTP_SOC]]

Prepares the inception_v3 assets for tutorial examples.

required arguments:
  -a ASSETS_DIR, --assets_dir ASSETS_DIR
                        directory containing the inception_v3 assets (default: None)

optional arguments:
  -d, --download        Download inception_v3 assets to inception_v3 example directory (default: False)
  -r {cpu,gpu,dsp,aip,all}, --runtime {cpu,gpu,dsp,aip,all}
                        Choose a runtime to set up tutorial for. 'all' option is only supported with --udo flag (default: cpu)
  -u, --udo             Generate and compile a user-defined operation package to be used with inception_v3. Softmax is simulated as a UDO for this script. Currently NOT supported on Windows (default: False)
  -l [HTP_SOC], --htp_soc [HTP_SOC]
                        Specify SOC target for generating HTP Offline Cache. For example: "--htp_soc sm8450" for waipio, default value is sm8550 if no value specified (default: None)
```

# `show_inceptionv3_classifications.py`

```
usage: show_inceptionv3_classifications.py [-h] -i INPUT_LIST -o OUTPUT_DIR -l LABELS_FILE [-v]

Display inception v3 classification results.

optional arguments:
  -h, --help            show this help message and exit
  -i INPUT_LIST, --input_list INPUT_LIST
                        File containing input list used to generate output_dir.
  -o OUTPUT_DIR, --output_dir OUTPUT_DIR
                        Output directory containing Result_X/prob.raw files matching input_list.
  -l LABELS_FILE, --labels_file LABELS_FILE
                        Path to ilsvrc_2012_labels.txt
  -v, --verbose_results
                        Display top 5 classifications
```

# `snpe-platform-validator`

```
DESCRIPTION:
snpe-platform-validator is a tool to check the capabilities of a device.
------------
ARGUMENTS:
-------------------
  --runtime <RUNTIME>   Specify the runtime to validate. <RUNTIME> : gpu, dsp, aip, all
  --coreVersion         Query the runtime core descriptor.
  --libVersion          Query the runtime core library API.
  --testRuntime         Run diagnostic tests on the specified runtime.
  --targetPath <DIR>    The directory to save output to. Defaults to /data/local/tmp/platformValidator.
  --debug               Turn on verbose logging.
  --help                Show this help message.
```

# `snpe_bench.py`

```
usage: snpe_bench.py [-h] -c CONFIG_FILE [-o OUTPUT_BASE_DIR_OVERRIDE]
                     [-v DEVICE_ID_OVERRIDE] [-r HOST_NAME] [-a]
                     [-t DEVICE_OS_TYPE_OVERRIDE] [-d] [-s SLEEP]
                     [-b USERBUFFER_MODE] [-p PERFPROFILE] [-l PROFILINGLEVEL]
                     [-json] [-cache]

Run the snpe_bench

required arguments:
  -c CONFIG_FILE, --config_file CONFIG_FILE
                        Path to a valid config file
                        Refer to sample config file config_help.json for more
                        detail on how to fill params in config file

optional arguments:
  -o OUTPUT_BASE_DIR_OVERRIDE, --output_base_dir_override OUTPUT_BASE_DIR_OVERRIDE
                        Sets the output base directory.
  -v DEVICE_ID_OVERRIDE, --device_id_override DEVICE_ID_OVERRIDE
                        Use this device ID instead of the one supplied in config
                        file. Cannot be used with -a
  -r HOST_NAME, --host_name HOST_NAME
                        Hostname/IP of remote machine to which devices are
                        connected.
  -a, --run_on_all_connected_devices_override
                        Runs on all connected devices, currently only support 1.
                        Cannot be used with -v
  -t DEVICE_OS_TYPE_OVERRIDE, --device_os_type_override DEVICE_OS_TYPE_OVERRIDE
                        Specify the target OS type, valid options are
                        ['android-aarch64', 'le_oe_gcc8.2', 'le64_oe_gcc8.2']
  -d, --debug           Set to turn on debug log
  -s SLEEP, --sleep SLEEP
                        Set number of seconds to sleep between runs e.g. 20
                        seconds
  -b USERBUFFER_MODE, --userbuffer_mode USERBUFFER_MODE
                        [EXPERIMENTAL] Enable user buffer mode, default to
                        float, can be tf8exact0
  -p PERFPROFILE, --perfprofile PERFPROFILE
                        Set the benchmark operating mode (balanced, default,
                        sustained_high_performance, high_performance,
                        power_saver, low_power_saver, high_power_saver,
                        extreme_power_saver, low_balanced, system_settings)
  -l PROFILINGLEVEL, --profilinglevel PROFILINGLEVEL
                        Set the profiling level mode (off, basic, moderate, detailed, linting).
                        Default is basic.
  -json, --generate_json
                        Set to produce json output.
  -cache, --enable_init_cache
                        Enable init caching mode to accelerate the network
                        building process. Defaults to disable.
```