# githubactions

This project will use githun action to build, containerise, push to EKS then Deploy 

To run from EKS without a load balancer,  we use port fowarding by running:

kubectl port-forward svc/sample-service 1234:5000


We can now hit  localhost:1234
