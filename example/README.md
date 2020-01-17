# Role example
With an infrastructure deployed you can run the actions via
the example site files.

With a valid `KUBECONFIG` environment variable and from within a
suitable Python virtual environment environment...

    $ kubectl create -f namespace.yaml
    
    $ ansible-playbook site-create-user-db.yaml
    $ ansible-playbook site-alter-user-db.yaml 
    $ ansible-playbook site-delete-user-db.yaml
    
    $ kubectl delete -f namespace.yaml

>   The example assumes the infrastructure namespace is
    `im-infra`. If it isn't then provide the correct value.

---
