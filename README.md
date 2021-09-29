##Nuances to ApplicationSet Generators

* unable to set cascade options. Will always be cascade=true


#Cluster Generator
Use Case: I have a single application I want to deploy multiple clusters

1 Application -> 0..N Clusters  

Parameters:  
* {{name}} - name of the cluster within ArgoCD  
* {{server}} - cluster endpoint  

Can add additional parameters via:  

* metadata.labels.<key> (for each label in the Secret)  
* metadata.annotations.<key> (for each annotation in the Secret)  

Considerations: Managed via labels in a cluster specific secret.

# Git Generator - Directory

Use Case: I have a multiple applications I want to deploy to a single cluster

1..N Applications -> 1 Cluster  

Considerations:  
If deploying multiple Applications you can only do the following:
All applications into 1 namespace

For example:  

      destination:
        server: https://kubernetes.default.svc
        namespace: mytestproject

Or deploy an application into a namespace derived from the basename

      destination:
        server: https://kubernetes.default.svc
        namespace: '{{path.basename}}'  

Or

      destination:
        server: https://kubernetes.default.svc
        namespace: '{{path.basename}}-httpd'  
Parameters:  
* {{path}}: The directory paths within the Git repository that match the path wildcard.
* {{path.basename}}: For any directory path within the Git repository that matches the path wildcard, the right-most path name is extracted (e.g. /directory/directory2 would produce directory2).


# Git Generator - File  