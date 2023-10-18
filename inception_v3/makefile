SHELL = /usr/bin/bash

################################################################################
# Environment Variables
################################################################################

# NOTE: change the following SNPE_VERSION and CONDA string to match your own setup
SNPE_VERSION := 2.14.2.230905
CONDA := /home/$(USER)/.conda/envs/xrbench

export SNPE_ROOT := /opt/qcom/aistack/snpe/$(SNPE_VERSION)
export PYTHONPATH := $(SNPE_ROOT)/lib/python/
export TENSORFLOW_HOME := $(CONDA)/lib/python3.8/site-packages/tensorflow
export HEXAGON_SDK_ROOT := /opt/qcom/Hexagon_SDK/3.5.4
export ANDROID_NDK_ROOT := /opt/android/android-sdk/ndk/26.0.10792818
export LD_LIBRARY_PATH := $(SNPE_ROOT)/lib/x86_64-linux-clang
export PATH := $(CONDA)/bin:$(SNPE_ROOT)/bin/x86_64-linux-clang:${ANDROID_NDK_ROOT}:$(CONDA)/lib/python3.8/site-packages/tensorflow/python/tools:${PATH}

MODEL_PATH = $(SNPE_ROOT)/examples/Models/inception_v3
MODEL_DLC = inception_v3_quantized.dlc

################################################################################
# Prepare model
################################################################################

prepare_model:
	python3 $(SNPE_ROOT)/examples/Models/inception_v3/scripts/setup_inceptionv3.py --download --htp_soc sm8450 --assets_dir ./assets --runtime dsp


################################################################################
# Test model on host machine (local machine)
################################################################################

test_model_on_host:
	rm -rf output
	snpe-net-run --container $(MODEL_PATH)/dlc/$(MODEL_DLC) --input_list $(MODEL_PATH)/data/cropped/raw_list.txt
	python3 $(MODEL_PATH)/scripts/show_inceptionv3_classifications.py -i $(MODEL_PATH)/data/cropped/raw_list.txt -l $(MODEL_PATH)/data/imagenet_slim_labels.txt -o output


################################################################################
# Prepare device
################################################################################

DEVICE_PATH = /data/local/tmp/snpe/$(SNPE_VERSION)

export SNPE_TARGET_ARCH := aarch64-android
export SNPE_TARGET_STL := libc++_shared.so
export SNPE_TARGET_DSPARCH := hexagon-v69

_copy_runtime_and_lib:
	adb shell "mkdir -p $(DEVICE_PATH)/bin"
	adb shell "mkdir -p $(DEVICE_PATH)/lib"
	adb shell "mkdir -p $(DEVICE_PATH)/dsp/lib"
	adb push $(SNPE_ROOT)/lib/$(SNPE_TARGET_ARCH)/$(SNPE_TARGET_STL) $(DEVICE_PATH)/lib
	adb push $(SNPE_ROOT)/lib/$(SNPE_TARGET_ARCH)/*.so $(DEVICE_PATH)/lib
	adb push $(SNPE_ROOT)/lib/$(SNPE_TARGET_DSPARCH)/unsigned/*.so $(DEVICE_PATH)/dsp/lib
	adb push $(SNPE_ROOT)/bin/$(SNPE_TARGET_ARCH)/snpe-net-run $(DEVICE_PATH)/bin
	adb push $(SNPE_ROOT)/bin/$(SNPE_TARGET_ARCH)/snpe-platform-validator $(DEVICE_PATH)/bin
	adb shell "chmod 777 $(DEVICE_PATH)/bin/snpe-net-run"
	adb shell "chmod 777 $(DEVICE_PATH)/bin/snpe-platform-validator"

_copy_model:
	adb shell "mkdir -p $(DEVICE_PATH)/inception_v3"
	adb push $(MODEL_PATH)/data/cropped $(DEVICE_PATH)/inception_v3/cropped
	adb push $(MODEL_PATH)/data/target_raw_list.txt $(DEVICE_PATH)/inception_v3
	adb push $(MODEL_PATH)/dlc/$(MODEL_DLC) $(DEVICE_PATH)/inception_v3

prepare_device: _copy_runtime_and_lib _copy_model

clean_device:
	adb shell "rm -rf $(DEVICE_PATH)"

################################################################################
# Validate device
################################################################################

SETUP_PATH := LD_LIBRARY_PATH=$(DEVICE_PATH)/lib ADSP_LIBRARY_PATH=$(DEVICE_PATH)/dsp/lib

validate_device:
	adb shell "$(SETUP_PATH) $(DEVICE_PATH)/bin/snpe-platform-validator --runtime gpu --testRuntime --coreVersion --libVersion --debug"
	adb shell "$(SETUP_PATH) $(DEVICE_PATH)/bin/snpe-platform-validator --runtime dsp --testRuntime --coreVersion --libVersion --debug"
	adb shell "$(SETUP_PATH) $(DEVICE_PATH)/bin/snpe-platform-validator --runtime aip --testRuntime --coreVersion --libVersion --debug"


################################################################################
# Run model on device
################################################################################

run_model_on_device_cpu:
	adb shell "cd $(DEVICE_PATH)/inception_v3 \
		&& $(SETUP_PATH) $(DEVICE_PATH)/bin/snpe-net-run --container $(MODEL_DLC) --input_list target_raw_list.txt" --perf_profile=default

run_model_on_device_gpu:
	adb shell "cd $(DEVICE_PATH)/inception_v3 \
		&& $(SETUP_PATH) $(DEVICE_PATH)/bin/snpe-net-run --container $(MODEL_DLC) --input_list target_raw_list.txt --use_gpu"

run_model_on_device_dsp:
	adb shell "cd $(DEVICE_PATH)/inception_v3 \
		&& $(SETUP_PATH) $(DEVICE_PATH)/bin/snpe-net-run --container $(MODEL_DLC) --input_list target_raw_list.txt --use_dsp"

run_model_on_device_aip:
	adb shell "cd $(DEVICE_PATH)/inception_v3 \
		&& $(SETUP_PATH) $(DEVICE_PATH)/bin/snpe-net-run --container $(MODEL_DLC) --input_list target_raw_list.txt --use_aip"

benchmark_model_on_device:
	python3 $(SNPE_ROOT)/benchmarks/SNPE/snpe_bench.py -c benchmark.json -a -o output
