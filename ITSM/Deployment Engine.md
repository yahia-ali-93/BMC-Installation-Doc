# Installation Instructions
Follow the documentation normally but consider the following:
- After you first unzip the deployment engine and update it's config file, names of files in the config file are usually wrong .. so not only you need to update the path .. you also need to update the file name.


### Pipeline parameters considerations

- `HELIX_PLATFORM_CUSTOMER_NAME`: The company name inside `infra.config`
- `PLATFORM_ADMIN_PLATFORM_EXTERNAL_IPS`: The external IP of a worker node
- `IMAGESECRET_NAME`: Just use the name in the example
- `REGISTRY_TYPE`: Always select **DTR** even if you're using a local harbor repository
- `PRODUCT-DEPLOY`: This section contains all the pipelines that will run. If you're doing a fresh installation and one step has succeeded before then uncheck it when you run the pipeline again
    - This is just to save time but no harm will come from leaving all as checked everytime
- `LOGS_ELASTICSEARCH_HOSTNAME`: Use the example in the documentation
- `FTS_ELASTICSEARCH_HOSTNAME`: Use the example in the documentation