SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='harbor-repo.vmware.com/tapsme/supply-chain/react-demo-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
    'react-demo',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null" +
               " && kubectl get workload react-demo --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['package.json'],
    container_selector='workload',
    live_update=[
      sync('./src', '/workspace/src'),
      sync('./public', '/workspace/public'),
      sync('package.json', '/workspace/package.json'),
      sync('package-lock.json', '/workspace/package-lock.json'),
    ]
)

k8s_resource('react-demo', port_forwards=["3000:3000"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'react-demo'}])

allow_k8s_contexts('pewpew-admin@pewpew')