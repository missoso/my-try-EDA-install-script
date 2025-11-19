# my-try-EDA-install-script

Goal is to install EDA in the fastest way possible and link it with a LLM key (from open.ai) and with the clab connector thus allowing a container lab topology to be connected directly to the EDA instance

1 - Try-EDA with a fine tune to allow it to work with the Containerlab EDA Connector Tool
2 - Install kubectl (if not already installed)
3 - EDA license
4 - Containerlab EDA Connector Tool instalation

 

# try-EDA

https://docs.eda.dev/25.8/getting-started/try-eda/#try-eda

But before you run the "make-eda" (the final step after all the downloads are done) there is one step required so that it will work with the clab connector 

Edit the prefs.mk and set simulate to false ! 

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

In the following file edit the name and add the license on a single line to the field "data" and paste the file contantents in the  CLI

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

confirm it is applied

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

Use the integrate option with the paramaentes 

```bash
:~/eda-topologies$ clab-connector integrate \
  --topology-data $path to the topology-data.json file of the container lab topology that is to be integrated\
  --eda-url https://$EDA_IP$:9443 \
  --eda-user $user \
  --eda-password $password
```


