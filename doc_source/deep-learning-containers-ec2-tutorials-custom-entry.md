# Custom Entrypoints<a name="deep-learning-containers-ec2-tutorials-custom-entry"></a>

For some images, AWS containers use a custom entrypoint script\. If you want to use your own entrypoint, you can override the entrypoint as follows\.
+ You can specify a custom entrypoint script to run\.

  ```
  docker run --entrypoint=/path/to/custom_entrypoint_script -it <image> /bin/bash
  ```
+ Alternatively, you can set the entrypoint to be empty\.

  ```
  docker run --entrypoint="" <image> /bin/bash
  ```