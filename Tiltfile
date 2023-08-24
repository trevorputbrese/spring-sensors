allow_k8s_contexts('eduk8s')
allow_k8s_contexts('aks-eus-tap-v1-6-cluster-3')

SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='your-registry.io/project/spring-sensors-iterate-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
    'spring-sensors-iterate',
    apply_cmd="tanzu apps workload apply -f config/workload-iterate.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null" +
               " && kubectl get workload spring-sensors-iterate --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload-iterate.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('spring-sensors-iterate', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'spring-sensors-iterate'}])