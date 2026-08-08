# pipeline-cicd-with-k8s

# CD with kubernetes - Automated Rolling deployments
A follow-up to "CD without kubernetes" project, demonstrating how the same pipeline evolves when kubernetes takes 
over container orchestration.

## Stack
- Node.js( minimal HTTP sever app)
- DOCKER
- Kubernetes( kind)
- Github Actions ( self-hosted runner)

### Pipeline architecture 
1. **`test` job** ( Github-hosted runner): syntax validation of the code
2. **`build-and-push` job** (self-hosted runner): builds the docker image,tags it with commit SHA, pushes it to dockerhub
3. **`deploy` job** (self-hosted runner): triggers `kubectl set image` to update the kubernetes deployment with the new image tag

Kubernetes then handles the rolling update itsel -- replacing pods gradually, with zero downtimw

## key difference from the non-kubernetes version
***whithout k8s***              #k8s=kubernetes
- the pipeline itself may restart the container(due docker run command)
-Downtime
- rollback is manual

***whith k8s
- kubernetes automatically restart container( rolling update)
- zero Downtime
-kubectl rollout undo

## Running the project locally
\`\`\`bash
kubectl apply -f deployment.yaml
kubectl get pods
kubectl port-forward deplpyment/mon-app 3000:3000
curl http://localhost:3000

## what this project demonstrates
- kubernetes Deployments and rolling updates
- decoupling build/push from deployement cia a container registry
- self-hosted runner integration with a local kubernetes cluster (kind)
- CI/CD pipeline designn with sequential job dependancies(`needs`)

# Related project
part of a broader Devops learning series - see [CD Without kubernetes](https://github.com/Lmntrixo/pipeline-ci-cd.git) for the baseline comparison.
