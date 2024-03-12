# ejson password setup

```
brew tap shopify/shopify
brew install ejson
```

```
mkdir -p /opt/ejson/keys
sudo chown -R $(whoami) /opt/ejson
```


Copy `SECRET_KEY` and paste it in the `/opt/ejson/keys/58c59e084396b5f49bc9d6a0b09967bb3b86534535da264ef723ced276548021`

```
chmod 777 /opt/ejson/keys/58c59e084396b5f49bc9d6a0b09967bb3b86534535da264ef723ced276548021
ejson decrypt passwords.ejson > passwords.json
chmod 440 /opt/ejson/keys/58c59e084396b5f49bc9d6a0b09967bb3b86534535da264ef723ced276548021
```

Try to run log-reader
