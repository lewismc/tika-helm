tika-helm
=========

[![Artifact HUB](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/apache-tika)](https://artifacthub.io/packages/helm/apache-tika/tika)
![Version: 3.2.3](https://img.shields.io/badge/Version-3.2.3-informational?style=flat-square)
![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square)
![AppVersion: 3.2.3.0-full](https://img.shields.io/badge/AppVersion-3.2.3.0--full-informational?style=flat-square)

<div class="artifacthub-widget" data-url="https://artifacthub.io/packages/helm/apache-tika/tika" data-theme="dark" data-header="true" data-stars="true" data-responsive="true"><blockquote><p lang="en" dir="ltr"><b>tika</b>: The official Helm chart for Apache Tika</p>&mdash; Open in <a href="https://artifacthub.io/packages/helm/apache-tika/tika">Artifact Hub</a></blockquote></div>

<img src="https://tika.apache.org/tika.png" width="300" />

We recommend that the Helm chart version is aligned to the version Tika (and subsequently the
version of the [Tika Docker image][]) you want to deploy.
This will ensure that you using a chart version that has been tested against the corresponding
production version. This will also ensure that the documentation and examples for the chart
will work with the version of Tika you are installing.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Installing](#installing)
  - [Install released version using Helm repository](#install-released-version-using-helm-repository)
  - [Install development version using main branch](#install-development-version-using-main-branch)
- [Upgrading](#upgrading)
- [Values](#values)
- [Testing](#testing)
- [Contributing](#contributing)
- [More Information](#more-information)
- [License](#license)
- [Maintainers](#maintainers)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
<!-- Use this to update TOC: -->
<!-- docker run --rm -it -v $(pwd):/usr/src jorgeandrada/doctoc --github -->

## Installing

### Install released version using Helm repository

**N.B.** You may or may not need/wish to install the chart into a specific **namespace**,
in which case you may need to augment the commands below.

* Add the Tika Helm charts repo:
`helm repo add tika https://apache.jfrog.io/artifactory/tika`

* Install it:
  - with Helm 3: `helm install tika tika/tika --set image.tag=${release.version} -n tika-test`, you will see something like
```
helm install tika tika/tika --set image.tag=latest-full -n tika-test

...
NAME: tika
LAST DEPLOYED: Mon Jan 24 13:38:01 2022
NAMESPACE: tika-test
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace tika-test -l "app.kubernetes.io/name=tika,app.kubernetes.io/instance=tika" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace tika-test $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:9998 to use your application"
  kubectl --namespace tika-test port-forward $POD_NAME 9998:$CONTAINER_PORT
```
You may notice that the _kubectl port forwarding_ experiences a _timeout issue_ which ultimately kills the app. In this case you can run port formarding in a loop
```
while true; do kubectl --namespace tika-test port-forward $POD_NAME 9998:$CONTAINER_PORT ; done
```
... this should keep `kubectl` reconnecting on connection lost.

### Install development version using main branch

* Clone the git repo: `git clone git@github.com:apache/tika-helm.git`

* Install it:
  - with Helm 3: `helm install tika . --set image.tag=latest-full`

## Upgrading

Please check `artifacthub.io/changes` in `Chart.yaml` before upgrading.

## Values

<table>
	<thead>
		<tr>
		<th>Key</th>
		<th>Type</th>
		<th>Default</th>
		<th>Description</th>
		<th>Example</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>additionalConfigs</td>
			<td>object</td>
			<td><pre lang="json">
{}
</pre>
</td>
			<td>Additional config files mounted alongside tika-config (e.g. custom MIME types). Keys are filenames, values are file contents. Use together with tikaConfig when your config references them (e.g. mime-table-path).</td>
			<td><pre lang="text">{"custom-mimetypes.xml": "<?xml ...><mime-info>...</mime-info>"}</pre></td>
		</tr>
		<tr>
			<td>affinity</td>
			<td>object</td>
			<td><pre lang="json">
{}
</pre>
</td>
			<td>Affinity rules for pod scheduling</td>
			<td><pre lang="text">{}, {podAntiAffinity: {...}}</pre></td>
		</tr>
		<tr>
			<td>autoscaling.apiVersion</td>
			<td>string</td>
			<td><pre lang="json">
"autoscaling/v2"
</pre>
</td>
			<td>API version for the HorizontalPodAutoscaler</td>
			<td><pre lang="text">"autoscaling/v2", "autoscaling/v2beta2"</pre></td>
		</tr>
		<tr>
			<td>autoscaling.enabled</td>
			<td>bool</td>
			<td><pre lang="json">
false
</pre>
</td>
			<td>Enable autoscaling for Tika pods</td>
			<td><pre lang="text">true, false</pre></td>
		</tr>
		<tr>
			<td>autoscaling.maxReplicas</td>
			<td>int</td>
			<td><pre lang="json">
100
</pre>
</td>
			<td>Maximum number of replicas</td>
			<td><pre lang="text">100, 10</pre></td>
		</tr>
		<tr>
			<td>autoscaling.minReplicas</td>
			<td>int</td>
			<td><pre lang="json">
1
</pre>
</td>
			<td>Minimum number of replicas</td>
			<td><pre lang="text">1, 2</pre></td>
		</tr>
		<tr>
			<td>autoscaling.targetCPUUtilizationPercentage</td>
			<td>int</td>
			<td><pre lang="json">
80
</pre>
</td>
			<td>Target CPU utilization percentage for autoscaling</td>
			<td><pre lang="text">80, 70</pre></td>
		</tr>
		<tr>
			<td>autoscaling.targetMemoryUtilizationPercentage</td>
			<td>int</td>
			<td><pre lang="json">
80
</pre>
</td>
			<td>Target memory utilization percentage for autoscaling</td>
			<td><pre lang="text">80, 85</pre></td>
		</tr>
		<tr>
			<td>config.base_url</td>
			<td>string</td>
			<td><pre lang="json">
"http://localhost/"
</pre>
</td>
			<td>Base URL for the Tika service</td>
			<td><pre lang="text">"http://localhost/", "https://tika.example.com/"</pre></td>
		</tr>
		<tr>
			<td>fullnameOverride</td>
			<td>string</td>
			<td><pre lang="json">
""
</pre>
</td>
			<td>Override the full name of the release</td>
			<td><pre lang="text">"", "tika-server"</pre></td>
		</tr>
		<tr>
			<td>image.pullPolicy</td>
			<td>string</td>
			<td><pre lang="json">
"IfNotPresent"
</pre>
</td>
			<td>Image pull policy for the Tika container</td>
			<td><pre lang="text">"IfNotPresent", "Always", "Never"</pre></td>
		</tr>
		<tr>
			<td>image.repository</td>
			<td>string</td>
			<td><pre lang="json">
"apache/tika"
</pre>
</td>
			<td>Docker image repository for Apache Tika</td>
			<td><pre lang="text">"apache/tika", "my-registry.io/apache/tika"</pre></td>
		</tr>
		<tr>
			<td>image.tag</td>
			<td>string</td>
			<td><pre lang="json">
"3.2.3.0-full"
</pre>
</td>
			<td>Overrides the image tag whose default is the chart appVersion</td>
			<td><pre lang="text">"3.2.3.0-full", "latest-full"</pre></td>
		</tr>
		<tr>
			<td>imagePullSecrets</td>
			<td>list</td>
			<td><pre lang="json">
[]
</pre>
</td>
			<td>Secrets for pulling images from a private registry</td>
			<td><pre lang="text">[], or [{name: "my-registry-secret"}]</pre></td>
		</tr>
		<tr>
			<td>ingress.annotations</td>
			<td>object</td>
			<td><pre lang="json">
{}
</pre>
</td>
			<td>Annotations for the ingress resource</td>
			<td><pre lang="text">{}, {kubernetes.io/ingress.class: "nginx"}</pre></td>
		</tr>
		<tr>
			<td>ingress.enabled</td>
			<td>bool</td>
			<td><pre lang="json">
false
</pre>
</td>
			<td>Enable ingress for the Tika service</td>
			<td><pre lang="text">true, false</pre></td>
		</tr>
		<tr>
			<td>ingress.hosts[0]</td>
			<td>object</td>
			<td><pre lang="json">
{
  "host": "chart-example.local",
  "paths": []
}
</pre>
</td>
			<td>Hostnames for the ingress</td>
			<td><pre lang="text">[{host: "tika.example.com", paths: [{path: "/", pathType: "Prefix"}]}]</pre></td>
		</tr>
		<tr>
			<td>ingress.tls</td>
			<td>list</td>
			<td><pre lang="json">
[]
</pre>
</td>
			<td>TLS configuration for the ingress</td>
			<td><pre lang="text">[], [{secretName: "tika-tls", hosts: ["tika.example.com"]}]</pre></td>
		</tr>
		<tr>
			<td>livenessProbe.failureThreshold</td>
			<td>int</td>
			<td><pre lang="json">
20
</pre>
</td>
			<td>Number of failed liveness probes before restarting the pod</td>
			<td><pre lang="text">20, 6</pre></td>
		</tr>
		<tr>
			<td>livenessProbe.initialDelaySeconds</td>
			<td>int</td>
			<td><pre lang="json">
15
</pre>
</td>
			<td>Initial delay before starting liveness probe (seconds)</td>
			<td><pre lang="text">15, 30</pre></td>
		</tr>
		<tr>
			<td>livenessProbe.periodSeconds</td>
			<td>int</td>
			<td><pre lang="json">
5
</pre>
</td>
			<td>Interval between liveness probes (seconds)</td>
			<td><pre lang="text">5, 10</pre></td>
		</tr>
		<tr>
			<td>livenessProbe.scheme</td>
			<td>string</td>
			<td><pre lang="json">
"HTTP"
</pre>
</td>
			<td>Scheme for liveness probe (HTTP or HTTPS)</td>
			<td><pre lang="text">"HTTP", "HTTPS"</pre></td>
		</tr>
		<tr>
			<td>livenessProbe.timeoutSeconds</td>
			<td>int</td>
			<td><pre lang="json">
30
</pre>
</td>
			<td>Timeout for liveness probe (seconds)</td>
			<td><pre lang="text">30, 10</pre></td>
		</tr>
		<tr>
			<td>nameOverride</td>
			<td>string</td>
			<td><pre lang="json">
""
</pre>
</td>
			<td>Override the name of the chart</td>
			<td><pre lang="text">"", "my-tika"</pre></td>
		</tr>
		<tr>
			<td>namespaceOverride</td>
			<td>string</td>
			<td><pre lang="json">
""
</pre>
</td>
			<td>Override the namespace for the release</td>
			<td><pre lang="text">"", "tika-prod"</pre></td>
		</tr>
		<tr>
			<td>networkPolicy.allowExternal</td>
			<td>bool</td>
			<td><pre lang="json">
false
</pre>
</td>
			<td>Allow external traffic without requiring a "-client" label</td>
			<td><pre lang="text">true, false</pre></td>
		</tr>
		<tr>
			<td>networkPolicy.enabled</td>
			<td>bool</td>
			<td><pre lang="json">
false
</pre>
</td>
			<td>Create a network policy to restrict traffic to pods within the same namespace that include the label `<release>-client: true`.</td>
			<td><pre lang="text">true, false</pre></td>
		</tr>
		<tr>
			<td>nodeSelector</td>
			<td>object</td>
			<td><pre lang="json">
{}
</pre>
</td>
			<td>Node selector for pod scheduling</td>
			<td><pre lang="text">{}, {disktype: "ssd"}</pre></td>
		</tr>
		<tr>
			<td>podAnnotations</td>
			<td>object</td>
			<td><pre lang="json">
{}
</pre>
</td>
			<td>Annotations to add to the Tika pods</td>
			<td><pre lang="text">{}, {"prometheus.io/scrape": "true"}</pre></td>
		</tr>
		<tr>
			<td>podSecurityContext</td>
			<td>object</td>
			<td><pre lang="json">
{}
</pre>
</td>
			<td></td>
			<td></td>
		</tr>
		<tr>
			<td>readinessProbe.failureThreshold</td>
			<td>int</td>
			<td><pre lang="json">
20
</pre>
</td>
			<td>Number of failed readiness probes before marking pod as not ready</td>
			<td><pre lang="text">20, 6</pre></td>
		</tr>
		<tr>
			<td>readinessProbe.initialDelaySeconds</td>
			<td>int</td>
			<td><pre lang="json">
15
</pre>
</td>
			<td>Initial delay before starting readiness probe (seconds)</td>
			<td><pre lang="text">15, 30</pre></td>
		</tr>
		<tr>
			<td>readinessProbe.periodSeconds</td>
			<td>int</td>
			<td><pre lang="json">
5
</pre>
</td>
			<td>Interval between readiness probes (seconds)</td>
			<td><pre lang="text">5, 10</pre></td>
		</tr>
		<tr>
			<td>readinessProbe.scheme</td>
			<td>string</td>
			<td><pre lang="json">
"HTTP"
</pre>
</td>
			<td>Scheme for readiness probe (HTTP or HTTPS)</td>
			<td><pre lang="text">"HTTP", "HTTPS"</pre></td>
		</tr>
		<tr>
			<td>readinessProbe.timeoutSeconds</td>
			<td>int</td>
			<td><pre lang="json">
30
</pre>
</td>
			<td>Timeout for readiness probe (seconds)</td>
			<td><pre lang="text">30, 10</pre></td>
		</tr>
		<tr>
			<td>replicaCount</td>
			<td>int</td>
			<td><pre lang="json">
1
</pre>
</td>
			<td>Number of Tika pod replicas to deploy</td>
			<td><pre lang="text">1, 2, 3</pre></td>
		</tr>
		<tr>
			<td>resources.limits</td>
			<td>object</td>
			<td><pre lang="json">
{
  "cpu": "2",
  "memory": "2000Mi"
}
</pre>
</td>
			<td>Resource limits for the Tika container</td>
			<td><pre lang="text">{cpu: "2", memory: 2000Mi}</pre></td>
		</tr>
		<tr>
			<td>resources.requests</td>
			<td>object</td>
			<td><pre lang="json">
{
  "cpu": "1",
  "memory": "1500Mi"
}
</pre>
</td>
			<td>Resource requests for the Tika container</td>
			<td><pre lang="text">{cpu: "1", memory: 1500Mi}</pre></td>
		</tr>
		<tr>
			<td>securityContext.allowPrivilegeEscalation</td>
			<td>bool</td>
			<td><pre lang="json">
true
</pre>
</td>
			<td>Allow privilege escalation for the container</td>
			<td><pre lang="text">true, false</pre></td>
		</tr>
		<tr>
			<td>securityContext.capabilities.drop</td>
			<td>list</td>
			<td><pre lang="json">
[
  "ALL"
]
</pre>
</td>
			<td>Capabilities to drop for the container</td>
			<td><pre lang="text">"[ALL]", "[NET_RAW]"</pre></td>
		</tr>
		<tr>
			<td>securityContext.readOnlyRootFilesystem</td>
			<td>bool</td>
			<td><pre lang="json">
true
</pre>
</td>
			<td>Run container with read-only root filesystem</td>
			<td><pre lang="text">true, false</pre></td>
		</tr>
		<tr>
			<td>securityContext.runAsGroup</td>
			<td>int</td>
			<td><pre lang="json">
35002
</pre>
</td>
			<td>Group ID to run the container</td>
			<td><pre lang="text">35002, 1000</pre></td>
		</tr>
		<tr>
			<td>securityContext.runAsNonRoot</td>
			<td>bool</td>
			<td><pre lang="json">
true
</pre>
</td>
			<td>Run container as non-root user</td>
			<td><pre lang="text">true, false</pre></td>
		</tr>
		<tr>
			<td>securityContext.runAsUser</td>
			<td>int</td>
			<td><pre lang="json">
35002
</pre>
</td>
			<td>User ID to run the container</td>
			<td><pre lang="text">35002, 1000</pre></td>
		</tr>
		<tr>
			<td>service.port</td>
			<td>int</td>
			<td><pre lang="json">
9998
</pre>
</td>
			<td>Port for the Tika service</td>
			<td><pre lang="text">9998, 8080</pre></td>
		</tr>
		<tr>
			<td>service.type</td>
			<td>string</td>
			<td><pre lang="json">
"ClusterIP"
</pre>
</td>
			<td>Type of Kubernetes service to expose Tika</td>
			<td><pre lang="text">"ClusterIP", "LoadBalancer", "NodePort"</pre></td>
		</tr>
		<tr>
			<td>serviceAccount.annotations</td>
			<td>object</td>
			<td><pre lang="json">
{}
</pre>
</td>
			<td>Annotations to add to the service account</td>
			<td><pre lang="text">{}, {eks.amazonaws.com/role-arn: "arn:aws:iam::..."}</pre></td>
		</tr>
		<tr>
			<td>serviceAccount.create</td>
			<td>bool</td>
			<td><pre lang="json">
true
</pre>
</td>
			<td>Specifies whether a service account should be created</td>
			<td><pre lang="text">true, false</pre></td>
		</tr>
		<tr>
			<td>serviceAccount.name</td>
			<td>string</td>
			<td><pre lang="json">
""
</pre>
</td>
			<td>The name of the service account to use; if not set and create is true, a name is generated</td>
			<td><pre lang="text">"", "tika-sa"</pre></td>
		</tr>
		<tr>
			<td>tikaConfig</td>
			<td>string</td>
			<td><pre lang="json">
""
</pre>
</td>
			<td>Custom Tika configuration (tika-config.xml) as a multiline string. Use for parser config, MIME excludes, and other options. See [configuring Tika](https://tika.apache.org/2.9.1/configuring.html). Use with additionalConfigs to mount extra files (e.g. custom MIME types). Example in values.yaml (commented block below).</td>
			<td><pre lang="text">tikaConfig: | with XML <properties><parsers>...</parsers></properties>; pair with additionalConfigs for files like custom-mimetypes.xml</pre></td>
		</tr>
		<tr>
			<td>tolerations</td>
			<td>list</td>
			<td><pre lang="json">
[]
</pre>
</td>
			<td>Tolerations for pod scheduling</td>
			<td><pre lang="text">[], [{key: "dedicated", operator: "Equal", value: "tika", effect: "NoSchedule"}]</pre></td>
		</tr>
		<tr>
			<td>topologySpreadConstraints</td>
			<td>list</td>
			<td><pre lang="json">
[]
</pre>
</td>
			<td>Control how Pods are spread across the cluster</td>
			<td><pre lang="text">[], [{maxSkew: 1, topologyKey: "topology.kubernetes.io/zone", whenUnsatisfiable: "DoNotSchedule"}]</pre></td>
		</tr>
	</tbody>
</table>

## Testing

```
helm plugin install https://github.com/helm-unittest/helm-unittest.git
helm unittest .
```

See [helm-unittest][] for canonical documentation.

## Contributing

Please check [CONTRIBUTING][] before any contribution or for any questions
about our development and testing process.

## More Information

For more infomation on Apache Tika Server, go to the [Apache Tika Server documentation][].

For more information on Apache Tika, go to the official [Apache Tika][] project website.

For more information on the Apache Software Foundation, go to the [Apache Software Foundation][] website.

## License
The code is licensed permissively under the [Apache License v2.0][].

## Maintainers

| Name | Email | URL |
| ---- | ------ | --- |
| lewismc | <lewismc@apache.org> | <https://github.com/lewismc> |
| stijnbrouwers |  | <https://github.com/stijnbrouwers> |
| philipsoutham |  | <https://github.com/philipsoutham> |
| frascu |  | <https://github.com/frascu> |
| euven |  | <https://github.com/euven> |
| ps0uth |  | <https://github.com/ps0uth> |
| ahilmathew |  | <https://github.com/ahilmathew> |
| aidanthewiz |  | <https://github.com/aidanthewiz> |
| bartek |  | <https://github.com/bartek> |
| CiraciNicolo |  | <https://github.com/CiraciNicolo> |
| amalucelli |  | <https://github.com/amalucelli> |
| thatmlopsguy |  | <https://github.com/thatmlopsguy> |

[Apache License v2.0]: https://www.apache.org/licenses/LICENSE-2.0.html
[Apache Software Foundation]: http://apache.org
[Apache Tika]: https://tika.apache.org
[Apache Tika Server documentation]: https://cwiki.apache.org/confluence/display/TIKA/TikaServer
[BREAKING_CHANGES.md]: https://github.com/apache/tika-helm/blob/master/BREAKING_CHANGES.md
[CHANGELOG.md]: https://github.com/apache/tika-helm/blob/master/CHANGELOG.md
[CONTRIBUTING]: https://github.com/apache/tika/blob/main/CONTRIBUTING.md
[helm-unittest]: https://github.com/helm-unittest/helm-unittest
[Tika Docker image]: https://hub.docker.com/r/apache/tika/tags?page=1&ordering=last_updated
