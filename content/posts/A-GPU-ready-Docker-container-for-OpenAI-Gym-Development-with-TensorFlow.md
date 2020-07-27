---
title: A GPU ready Docker container for OpenAI Gym Development with TensorFlow
date: '2018-01-20T10:57:15.773Z'
subtitle: "Intro post to my Dockerfile, published and open for anyone to\_use"
excerpt: 'Intro post to my Dockerfile, published and open for anyone to use'
layout: post
---
> Update: I archived the project because I am no longer using it and the libraries are moving fast. Feel free to fork it and make it better! Let me know if you end up getting hundreds of stars on GitHub

* * *

So, you want to write an agent, competing in the OpenAI Gym, you want to use Keras or TensorFlow or something similar and you don’t want everything installed on your workstation? You have come to the right place!

This post outlines the concept behind the Github repository below

[**pascalwhoop/tf\_openailab\_gpu\_docker**  
*Contribute to tf\_openailab\_gpu\_docker development by creating an account on GitHub.*github.com](https://github.com/pascalwhoop/tf_openailab_gpu_docker "https://github.com/pascalwhoop/tf_openailab_gpu_docker")[](https://github.com/pascalwhoop/tf_openailab_gpu_docker)

**It’s** **for anyone who has a decent workstation and wants to get into deep reinforcement learning** without all the troubles of finding your way through the jungle of dependencies. I fought through nvidia-driver dependencies, missing libraries, python version mismatches, jupyter kernels reported wrongly, compiler dependencies missing etc. etc. Now, you don’t have to anymore.

**Because of the nvidia TOS, I cannot package this as an image, as the cudnn library may not be distributed by me. Sorry!**

The build of the image takes about 20 minutes on a 20MBit DSL and the size will be about 5GB in total. Yes, it’s that big. Maybe I’ll get it a bit lighter in the future, but the nvidia dependencies as well as the X and make-utils are quiet large.

### What’s included

*   Jupyter Notebooks with a Python3 kernel
*   OpenAI Gym
*   OpenAI Roboschool based on bullet3 (FOSS, not proprietary! yay)
*   TensorFlow-GPU
*   nvidia gpu access from the container
*   a lot of Python libraries for data science work as shown below
*   ffmpeg, X, xvfb for remote GUI access

    # python3 packages  
    jupyter pandas matplotlib scipy seaborn scikit-learn scikit-Image sympy cython patsy statsmodels cloudpickle dill numba bokeh tensorflow-gpu gym pyopengl

### How to get started

This is what you need to get started

*   Docker
*   NVidia GPU with CUDA (drivers)
*   Unix machine (I use Ubuntu 17.10), if you are on MacOS, I’d welcome some feedback on GitHub
*   cudnn library files, which require a registration with nvidia, more on that in the [TensorFlow docs](https://www.tensorflow.org/install/install_linux)

More details can be found on the [GitHub project page](https://github.com/pascalwhoop/tf_openailab_gpu_docker). If there is any issues or feedback, feel free to [create an issue](https://github.com/pascalwhoop/tf_openailab_gpu_docker/issues/new). I intend to merge this into some larger project, I am just not sure which one. Is it a Jupyter image or a TensorFlow or OpenAI? Let me know what you think!
