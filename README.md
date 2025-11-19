# my-try-EDA-install-script

Goal is to install EDA in the fastest way possible and link it with a LLM key (from open.ai) and with the clab connector thus allowing a container lab topology to be connected directly to the EDA instance

# try-EDA

![https://docs.eda.dev/25.8/getting-started/try-eda/#try-eda](https://docs.eda.dev/25.8/getting-started/try-eda/#try-eda)

But before you run the "make-eda" (the final step after all the downloads are done) there is one step required so that it will work with the clab connector 

Edit the prefs.mk and set simulate to false ! 

$ cat prefs.mk | grep -i false
# set to false when connecting hardware nodes to the cluster
# When set to false, the topology-load make target will be skipped and
SIMULATE := false

# ensure there are no trailing spaces after the text

