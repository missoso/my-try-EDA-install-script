# my-try-EDA-install-script

Goal is to install EDA and link it with the clab connector thus allowing a container lab topology to be connected directly to the EDA instance.

During the EDA installation it is possible to add the LLM key directly during the installation process, but it can also be added afterwards with a series of commands also detailed in this repository.

1 - Try-EDA with a fine tune to allow it to work with the Containerlab EDA Connector Tool

2 - Install kubectl (if not already installed)

3 - Add the EDA license

4 - Containerlab EDA Connector Tool installation

5 - Connect a containerlab topology to EDA

6 - (optional) Add a LLM key to EDA after the installation 

 

# try-EDA

https://docs.eda.dev/25.8/getting-started/try-eda/#try-eda

But before running the "make-eda" (the final step after all the downloads) there is one step required so that it will work with the clab connector 

Edit the prefs.mk and set simulate to false, and only run the "make-eda" afterwards

```bash
$ cat prefs.mk | grep -i false
# set to false when connecting hardware nodes to the cluster
# When set to false, the topology-load make target will be skipped and
SIMULATE := false

# ensure there are no trailing spaces after the text
```
# Install kubectl

https://kubernetes.io/docs/tasks/tools/

# Install the EDA license

In the following file edit the "name" (optional) and add the license on a single line to the field "data" and paste the file contents in the  CLI

```bash
cat << 'EOF' | kubectl apply -f -
apiVersion: core.eda.nokia.com/v1
kind: License
metadata:
  name: eda-non-prod-license-sep
  namespace: eda-system
spec:
  enabled: true
  data: ""
EOF
```

Confirm it is applied

```bash
:~/playground$ kubectl get -n eda-system license
NAME                       AGE
eda-non-prod-license-sep   
```

# Containerlab EDA Connector Tool instalation

https://github.com/eda-labs/clab-connector

```bash
:~/playground$ clab-connector --help
                                                                                                                              
 Usage: clab-connector [OPTIONS] COMMAND [ARGS]...                                                                            
                                                                                                                              
 Integrate or remove an existing containerlab topology with EDA (Event-Driven Automation)                                     
                                                                                                                              
╭─ Options ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│ --install-completion          Install completion for the current shell.                                                    │
│ --show-completion             Show completion for the current shell, to copy it or customize the installation.             │
│ --help                        Show this message and exit.                                                                  │
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
╭─ Commands ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│ integrate      Integrate containerlab with EDA                                                                             │
│ remove         Remove containerlab integration from EDA                                                                    │
│ export-lab     Export an EDA-managed topology from a namespace to a .clab.yaml file                                        │
│ generate-crs   Generate CR YAML manifests from a containerlab topology without applying them to EDA.                       │
│ check-sync     Check synchronization status of nodes in EDA                                                                │
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
```

Launch the container lab topology and use the integrate command to connect it to EDA (requires the path to the topology-data.json file of the container lab topology created)

```bash
:~/eda-topologies$ clab-connector integrate \
  --topology-data $path to the topology-data.json \
  --eda-url https://$EDA_IP$:9443 \
  --eda-user $user \
  --eda-password $password
```


# (Optional) Add LLm key to EDA
https://platform.openai.com

https://platform.openai.com/api-keys

```bash
kubectl get engineconfigs.core.eda.nokia.com engine-config -n eda-system -o yaml > cfgfull.txt

Open the file above and look for 

llm:
    apiKey: ""
    model: gpt-4o

Add the key to the field above


kubectl apply -f cfgfull.txt

kubectl get pods -n eda-system | grep eda-ce- # get the pod number

kubectl delete pod eda-ce-xxxxxxxx -n eda-system 

kubectl get pods -A

kubectl get engineconfigs.core.eda.nokia.com engine-config -n eda-system -o yaml | grep -i LLM -A 10

```

