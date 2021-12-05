Kubectl apply
  - kubectl apply -f filename.yml

Using kubectl apply
  - create/update resources in a file
    - kubectl apply -f myfile.yaml
  - create/update a whole directory of yaml
    - kubectl apply -f myyaml/
  - create/update from a URL
    - kubectl apply -f https://bret.run/pod.yml
  - Be careful, lets look at it first (browser or curl)
    - curl -L https://bret.run/pod
  - Win PoSH? start https://bret.run/pod.yml

Kubernetes Configuration YAML
  - Kubernetes configuration file (YAML or JSON)
  - Each file contains one or more manifests
  - Each manifest describes an API object (deployment, job, secret)
  - Each manifest needs four parts (root key:values in the file)
    - apiVersion:
    - kind:
    - metadata:
    - spec:

Building Your YAML Files
  - kind: We can get a list of resources the cluster supports
    - kubectl api-resources
  - Notice some resources have multiple API's (old vs. new)
  - apiVersion: We can get the API versions the cluster supports
    - kubectl api-versions
  - metadata: only name is required
  - spec: Where all the action is at!

Building Your YAML spec
  - We can get all the keys each kind supports
    - kubectl explain services --recursive
  - Oh boy! Let's slow down
    - kubectl explain services.spec
  - We can walk through the spec this way
    - kubectl explain services.spec.type
  - spec: can have sub spec: of other resources
    - kubectl explain deployment.spec.template.spec.volumes.nfs.server
  - We can also use docs
    - kubernetes.io/docs/reference/#api-reference

Dry Runs With Apply YAML
  - New stuff, not out of beta yet (1.15)
  - dry-run a create (client side only)
    - kubectl apply -f app.yml --dry-run
  - dry-run a create/update on server
    - kubectl apply -f app.yml --server-dry-run
  - see a diff visually
    - kubectl diff -f app.yml

Labels and Annotations
  - Labels goes under metadata: in your YAML
  - Simple list of key: value for identifying your resource later by selecting, grouping, or filtering for it
  - Common examples include tier: frontend, app: api, env: prod, customer: acme.co
  - Not meant to hold complex, large, or nonidentifying info, which is what annotations are for
  - filter a get command
    - kubectl get pods -l app=nginx
  - apply only matching labels
    - kubectl apply -f myfile.yaml -l app=nginx

Label Selectors
  - The "glue" telling Services and Deployments which pods are theirs
  - Many resources use Label Selectors to "link" resource dependencies
  - You'll see these match up in the Service and Deployment YAML
  - Use Labels and Selectors to control which pods go to which nodes
  - Taints and Tolerations also control node placement

---------------------------

External Notes

---------------------------

Understanding Kubernetes Objects : https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/
  - Understanding Kubernetes objects
    - Kubernetes objects are persistent entities in the Kubernetes system. Kubernetes uses these entities to represent the state of your cluster:
      - What containerized applications are running (and on which nodes)
      - The resources available to those applications
      - The policies around how those applications behave, such as restart policies, upgrades, and fault-tolerance
    - A Kubernetes object is a "record of intent"--once you create the object, the Kubernetes system will constantly work to ensure that object exists. By creating an object, you're effectively telling the Kubernetes system what you want your cluster's workload to look like; this is your cluster's desired state.
    - Object Spec and Status
      - Almost every Kubernetes object includes two nested object fields that govern the object's configuration: the object spec and the object status.
      - For objects that have a spec, you have to set this when you create the object, providing a description of the characteristics you want the resource to have: its desired state.
      - The status describes the current state of the object, supplied and updated by the Kubernetes system and its components. The Kubernetes control plane continually and actively manages every object's actual state to match the desired state you supplied.
    - Describing a Kubernetes object
      - When you create an object in Kubernetes, you must provide the object spec that describes its desired state, as well as some basic information about the object (such as a name).
      - When you use the Kubernetes API to create the object (either directly or via kubectl), that API request must include that information as JSON in the request body.
        - Most often, you provide the information to kubectl in a .yaml file. kubectl converts the information to JSON when making the API request.
      - Example :
      ```yaml
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: nginx-deployment
      spec:
        selector:
          matchLabels:
            app: nginx
        replicas: 2 # tells deployment to run 2 pods matching the template
        template:
          metadata:
            labels:
              app: nginx
          spec:
            containers:
            - name: nginx
              image: nginx:1.14.2
              ports:
              - containerPort: 80
      ```
      - One way to create a Deployment using a .yaml file like the one above is to use the kubectl apply command in the kubectl command-line interface, passing the .yaml file as an argument.
        - kubectl apply -f https://k8s.io/examples/application/deployment.yaml --record
    - Required Fields
      - In the .yaml file for the Kubernetes object you want to create, you'll need to set values for the following fields:
        - apiVersion - Which version of the Kubernetes API you're using to create this object
        - kind - What kind of object you want to create
        - metadata - Data that helps uniquely identify the object, including a name string, UID, and optional namespace
        - spec - What state you desire for the object
      - The precise format of the object spec is different for every Kubernetes object, and contains nested fields specific to that object.

APIServer dry-run and kubectl diff : https://kubernetes.io/blog/2019/01/14/apiserver-dry-run-and-kubectl-diff/
   - Declarative configuration management, also known as configuration-as-code, is one of the key strengths of Kubernetes. It allows users to commit the desired state of the cluster, and to keep track of the different versions, improve auditing and automation through CI/CD pipelines
   - Challenges
    - A few pieces are still missing in order to have a seamless declarative experience with Kubernetes, and we tried to address some of these:
      - While compilers and linters do a good job to detect errors in pull-requests for code, a good validation is missing for Kubernetes configuration files. The existing solution is to run kubectl apply --dry-run, but this runs a local dry-run that doesn't talk to the server: it doesn't have server validation and doesn't go through validating admission controllers. As an example, Custom resource names are only validated on the server so a local dry-run won't help.
      - It can be difficult to know how your object is going to be applied by the server for multiple reasons:
        - Defaulting will set some fields to potentially unexpected values,
        - Mutating webhooks might set fields or clobber/change some values.
        - Patch and merges can have surprising effects and result in unexpected objects. For example, it can be hard to know how lists are going to be ordered once merged.
  - APIServer dry-run
    - APIServer dry-run was implemented to address these two problems:
      - it allows individual requests to the apiserver to be marked as "dry-run",
      - the apiserver guarantees that dry-run requests won't be persisted to storage,
      - the request is still processed as typical request: the fields are defaulted, the object is validated, it goes through the validation admission chain, and through the mutating admission chain, and then the final object is returned to the user as it normally would, without being persisted.
    - While dynamic admission controllers are not supposed to have side-effects on each request, dry-run requests are only processed if all admission controllers explicitly announce that they don't have any dry-run side-effects
    - How to enable it :
      - Server-side dry-run is enabled through a feature-gate.
      - enabled/disabled using kube-apiserver --feature-gates DryRun=true
      - If you have dynamic admission controllers, you might have to fix them to:
        - Remove any side-effects when the dry-run parameter is specified on the webhook request,
        - Specify in the sideEffects field of the admissionregistration.k8s.io/v1beta1.Webhook object to indicate that the object doesn't have side-effects on dry-run (or at all).
    - How to use it
      - You can trigger the feature from kubectl by using kubectl apply --server-dry-run, which will decorate the request with the dryRun flag and return the object as it would have been applied, or an error if it would have failed.
  - Kubectl diff
    - APIServer dry-run is convenient because it lets you see how the object would be processed, but it can be hard to identify exactly what changed if the object is big. kubectl diff does exactly what you want by showing the differences between the current "live" object and the new "dry-run" object. It makes it very convenient to focus on only the changes that are made to the object, how the server has merged these and how the mutating webhooks affects the output.
    - How to use it
      - kubectl diff is meant to be as similar as possible to kubectl apply: kubectl diff -f some-resources.yaml will show a diff for the resources in the yaml file. One can even use the diff program of their choice by using the KUBECTL_EXTERNAL_DIFF environment variable, for example:
        - KUBECTL_EXTERNAL_DIFF=meld kubectl diff -f some-resources.yaml

Labels and Selectors
  - Labels are key/value pairs that are attached to objects, such as pods.
  - Labels are intended to be used to specify identifying attributes of objects that are meaningful and relevant to users, but do not directly imply semantics to the core system.
  - Labels can be used to organize and to select subsets of objects.
  - Labels can be attached to objects at creation time and subsequently added and modified at any time.
  - Each object can have a set of key/value labels defined.
  - Each Key must be unique for a given object.
  ```yaml
  "metadata": {
    "labels": {
      "key1" : "value1",
      "key2" : "value2"
    }
  }
  ```
  - Labels allow for efficient queries and watches and are ideal for use in UIs and CLIs. Non-identifying information should be recorded using annotations.
  - Motivation
    - Labels enable users to map their own organizational structures onto system objects in a loosely coupled fashion, without requiring clients to store these mappings.
    - Service deployments and batch processing pipelines are often multi-dimensional entities (e.g., multiple partitions or deployments, multiple release tracks, multiple tiers, multiple micro-services per tier).
    - Management often requires cross-cutting operations, which breaks encapsulation of strictly hierarchical representations, especially rigid hierarchies determined by the infrastructure rather than by users.
    - Example Labels :
      - "release" : "stable", "release" : "canary"
      - "environment" : "dev", "environment" : "qa", "environment" : "production"
      - "tier" : "frontend", "tier" : "backend", "tier" : "cache"
      - "partition" : "customerA", "partition" : "customerB"
      - "track" : "daily", "track" : "weekly"
  - Syntax and character set
    - Labels are key/value pairs. Valid label keys have two segments: an optional prefix and name, separated by a slash (/).
    - The name segment is required and must be 63 characters or less, beginning and ending with an alphanumeric character ([a-z0-9A-Z]) with dashes (-), underscores (_), dots (.), and alphanumerics between.
    - The prefix is optional. If specified, the prefix must be a DNS subdomain: a series of DNS labels separated by dots (.), not longer than 253 characters in total, followed by a slash (/).
    - If the prefix is omitted, the label Key is presumed to be private to the user. Automated system components (e.g. kube-scheduler, kube-controller-manager, kube-apiserver, kubectl, or other third-party automation) which add labels to end-user objects must specify a prefix.
    - Valid label value:
      - must be 63 characters or less (cannot be empty),
      - must begin and end with an alphanumeric character ([a-z0-9A-Z]),
      - could contain dashes (-), underscores (_), dots (.), and alphanumerics between.
  - Label selectors
    - Unlike names and UIDs, labels do not provide uniqueness. In general, we expect many objects to carry the same label(s).
    - Via a label selector, the client/user can identify a set of objects. The label selector is the core grouping primitive in Kubernetes.
    - The API currently supports two types of selectors: equality-based and set-based. A label selector can be made of multiple requirements which are comma-separated. In the case of multiple requirements, all must be satisfied so the comma separator acts as a logical AND (&&) operator.
    - The semantics of empty or non-specified selectors are dependent on the context, and API types that use selectors should document the validity and meaning of them.
    - Note: For some API types, such as ReplicaSets, the label selectors of two instances must not overlap within a namespace, or the controller can see that as conflicting instructions and fail to determine how many replicas should be present.
    - Equality-based requirement
      - Equality- or inequality-based requirements allow filtering by label keys and values. Matching objects must satisfy all of the specified label constraints, though they may have additional labels as well. Three kinds of operators are admitted =,==,!=. The first two represent equality (and are synonyms), while the latter represents inequality.
      - For example:
        ```bash
        environment = production
        tier != frontend
        ```
      - The former selects all resources with key equal to environment and value equal to production. The latter selects all resources with key equal to tier and value distinct from frontend, and all resources with no labels with the tier key. One could filter for resources in production excluding frontend using the comma operator: environment=production,tier!=frontend
    - Set-based requirement
      - Set-based label requirements allow filtering keys according to a set of values. Three kinds of operators are supported: in,notin and exists (only the key identifier).
      - For example:
        ```bash
        environment in (production, qa)
        tier notin (frontend, backend)
        partition
        !partition
        ```
        - The first example selects all resources with key equal to environment and value equal to production or qa.
        - The second example selects all resources with key equal to tier and values other than frontend and backend, and all resources with no labels with the tier key.
        - The third example selects all resources including a label with key partition; no values are checked.
        - The fourth example selects all resources without a label with key partition; no values are checked.
      - Similarly the comma separator acts as an AND operator. So filtering resources with a partition key (no matter the value) and with environment different than  qa can be achieved using partition,environment notin (qa).
  - API
    - LIST and WATCH filtering
      - LIST and WATCH operations may specify label selectors to filter the sets of objects returned using a query parameter. Both requirements are permitted (presented here as they would appear in a URL query string):
        - equality-based requirements: ?labelSelector=environment%3Dproduction,tier%3Dfrontend
        - set-based requirements: ?labelSelector=environment+in+%28production%2Cqa%29%2Ctier+in+%28frontend%29
      - Some Kubernetes objects, such as services and replicationcontrollers, also use label selectors to specify sets of other resources, such as pods.
    - Service and ReplicationController
      - The set of pods that a service targets is defined with a label selector. Similarly, the population of pods that a replicationcontroller should manage is also defined with a label selector.
      - Labels selectors for both objects are defined in json or yaml files using maps, and only equality-based requirement selectors are supported.

Labels and Annotations in Kubernetes : https://vsupalov.com/kubernetes-labels-annotations-difference/
  - Labels
    - Contain identifying information and are a used by selector queries or within selector sections in object definitions.
    - Queries can be evaluated quickly, using optimized data structures and algorithms.
  - Annotations
    - Are used for non-identifying information. Stuff not used internally by k8s.
    - You can’t specify selectors over them within Kubernetes, but they can be used by external tools and libraries.
    - As the internal performance of Kubernetes is not negatively impacted by huge annotations, the keys and values are not constrained like labels are.
  - Next Steps
    - If you need to point Kubernetes to a group of resources, labels are the way to go. Make sure that the data you have in mind abides by the constraints.
    - If you want to annotate your setup with data which will help people or tools, but not Kubernetes itself, it’s better to put it into annotation meta data.
    - Could your current workflows benefit from having direct access to something like timestamps, commit SHAs or the name of the person who is responsible for certain resources? Just add an annotation step to your current deployment pipeline.

Recommended Labels : https://kubernetes.io/docs/concepts/overview/working-with-objects/common-labels/
  - A common set of labels allows tools to work interoperably, describing objects in a common manner that all tools can understand.
  - In addition to supporting tooling, the recommended labels describe applications in a way that can be queried.
  - The metadata is organized around the concept of an application. Kubernetes is not a platform as a service (PaaS) and doesn't have or enforce a formal notion of an application. Instead, applications are informal and described with metadata. The definition of what an application contains is loose.
  - Shared labels and annotations share a common prefix: app.kubernetes.io. Labels without a prefix are private to users. The shared prefix ensures that shared labels do not interfere with custom user labels.
  - Labels
    <table>
      <thead>
        <tr>
          <th>Key</th>
          <th>Description</th>
          <th>Example</th>
          <th>Type</th>
        </tr>
      </thead>
      <tbody>
      <tr>
        <td><code>app.kubernetes.io/name</code></td>
        <td>The name of the application</td>
        <td><code>mysql</code></td>
        <td>string</td>
      </tr>
      <tr>
        <td><code>app.kubernetes.io/instance</code></td>
        <td>A unique name identifying the instance of an application</td>
        <td><code>mysql-abcxzy</code></td>
        <td>string</td>
      </tr>
      <tr>
        <td><code>app.kubernetes.io/version</code></td>
        <td>The current version of the application (e.g., a semantic version, revision hash, etc.)</td>
        <td><code>5.7.21</code></td>
        <td>string</td>
      </tr>
      <tr>
        <td><code>app.kubernetes.io/component</code></td>
        <td>The component within the architecture</td>
        <td><code>database</code></td>
        <td>string</td>
      </tr>
      <tr>
        <td><code>app.kubernetes.io/part-of</code></td>
        <td>The name of a higher level application this one is part of</td>
        <td><code>wordpress</code></td>
        <td>string</td>
      </tr>
      <tr>
        <td><code>app.kubernetes.io/managed-by</code></td>
        <td>The tool being used to manage the operation of an application</td>
        <td><code>helm</code></td>
        <td>string</td>
      </tr>
      </tbody>
    </table>
    - To illustrate these labels in action, consider the following StatefulSet object:

      ```yaml
      apiVersion: apps/v1
      kind: StatefulSet
      metadata:
        labels:
          app.kubernetes.io/name: mysql
          app.kubernetes.io/instance: mysql-abcxzy
          app.kubernetes.io/version: "5.7.21"
          app.kubernetes.io/component: database
          app.kubernetes.io/part-of: wordpress
          app.kubernetes.io/managed-by: helm
      ```
  - Applications And Instances Of Applications
    - An application can be installed one or more times into a Kubernetes cluster and, in some cases, the same namespace.
    - The name of an application and the instance name are recorded separately.
    - This enables the application and instance of the application to be identifiable.
  - Examples :
    - A Simple Stateless Service
      - The Deployment is used to oversee the pods running the application itself.
      ```yaml
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        labels:
          app.kubernetes.io/name: myservice
          app.kubernetes.io/instance: myservice-abcxzy
      ...
      ```
      - The Service is used to expose the application.
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        labels:
          app.kubernetes.io/name: myservice
          app.kubernetes.io/instance: myservice-abcxzy
      ...
      ```

Assigning Pods to Nodes : https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/
  - You can constrain a Pod so that it can only run on particular set of Node(s). There are several ways to do this and the recommended approaches all use label selectors to facilitate the selection.
  - There are some circumstances where you may want to control which node the pod deploys to - for example to ensure that a pod ends up on a machine with an SSD attached to it, or to co-locate pods from two different services that communicate a lot into the same availability zone.
  - nodeSelector
    - nodeSelector is the simplest recommended form of node selection constraint. nodeSelector is a field of PodSpec.
    - It specifies a map of key-value pairs. For the pod to be eligible to run on a node, the node must have each of the indicated key-value pairs as labels (it can have additional labels as well).
    - Steps :
      1. Attach label to the node
        - Run kubectl get nodes to get the names of your cluster's nodes. Pick out the one that you want to add a label to, and then run kubectl label nodes <node-name> <label-key>=<label-value> to add a label to the node you've chosen.
        - You can verify that it worked by re-running kubectl get nodes --show-labels and checking that the node now has a label. You can also use kubectl describe node "nodename" to see the full list of labels of the given node.
      2. Add a nodeSelector field to your pod configuration
        - Take whatever pod config file you want to run, and add a nodeSelector section to it.
        ```yaml
        apiVersion: v1
        kind: Pod
        metadata:
          name: nginx
          labels:
            env: test
        spec:
          containers:
          - name: nginx
            image: nginx
            imagePullPolicy: IfNotPresent
          nodeSelector:
            disktype: ssd
        ```
        - When you then run kubectl apply -f https://k8s.io/examples/pods/pod-nginx.yaml, the Pod will get scheduled on the node that you attached the label to. You can verify that it worked by running kubectl get pods -o wide and looking at the "NODE" that the Pod was assigned to.
  - Interlude: built-in node labels
    - In addition to labels you attach, nodes come pre-populated with a standard set of labels.
    - Note: The value of these labels is cloud provider specific and is not guaranteed to be reliable. For example, the value of kubernetes.io/hostname may be the same as the Node name in some environments and a different value in other environments.
  - Node isolation/restriction
    - Adding labels to Node objects allows targeting pods to specific nodes or groups of nodes.
    - This can be used to ensure specific pods only run on nodes with certain isolation, security, or regulatory properties.
    - When using labels for this purpose, choosing label keys that cannot be modified by the kubelet process on the node is strongly recommended. This prevents a compromised node from using its kubelet credential to set those labels on its own Node object, and influencing the scheduler to schedule workloads to the compromised node.
    - The NodeRestriction admission plugin prevents kubelets from setting or modifying labels with a node-restriction.kubernetes.io/ prefix. To make use of that label prefix for node isolation:
      - Ensure you are using the Node authorizer and have enabled the NodeRestriction admission plugin.
      - Add labels under the node-restriction.kubernetes.io/ prefix to your Node objects, and use those labels in your node selectors. For example, example.com.node-restriction.kubernetes.io/fips=true or example.com.node-restriction.kubernetes.io/pci-dss=true.
  - Affinity and anti-affinity
    - nodeSelector provides a very simple way to constrain pods to nodes with particular labels. The affinity/anti-affinity feature, greatly expands the types of constraints you can express. The key enhancements are
      1. The affinity/anti-affinity language is more expressive. The language offers more matching rules besides exact matches created with a logical AND operation;
      2. you can indicate that the rule is "soft"/"preference" rather than a hard requirement, so if the scheduler can't satisfy it, the pod will still be scheduled;
      3. you can constrain against labels on other pods running on the node (or other topological domain), rather than against labels on the node itself, which allows rules about which pods can and cannot be co-located
    - The affinity feature consists of two types of affinity, "node affinity" and "inter-pod affinity/anti-affinity". Node affinity is like the existing nodeSelector (but with the first two benefits listed above), while inter-pod affinity/anti-affinity constrains against pod labels rather than node labels, as described in the third item listed above, in addition to having the first and second properties listed above.
    - Node affinity
      - Node affinity is conceptually similar to nodeSelector -- it allows you to constrain which nodes your pod is eligible to be scheduled on, based on labels on the node.
      - There are currently two types of node affinity, called requiredDuringSchedulingIgnoredDuringExecution and preferredDuringSchedulingIgnoredDuringExecution.
      - You can think of them as "hard" and "soft" respectively, in the sense that the former specifies rules that must be met for a pod to be scheduled onto a node (similar to nodeSelector but using a more expressive syntax), while the latter specifies preferences that the scheduler will try to enforce but will not guarantee.
      - The "IgnoredDuringExecution" part of the names means that, similar to how nodeSelector works, if labels on a node change at runtime such that the affinity rules on a pod are no longer met, the pod continues to run on the node.
      - In the future we plan to offer requiredDuringSchedulingRequiredDuringExecution which will be identical to requiredDuringSchedulingIgnoredDuringExecution except that it will evict pods from nodes that cease to satisfy the pods' node affinity requirements.
        - Thus an example of requiredDuringSchedulingIgnoredDuringExecution would be "only run the pod on nodes with Intel CPUs" and an example preferredDuringSchedulingIgnoredDuringExecution would be "try to run this set of pods in failure zone XYZ, but if it's not possible, then allow some to run elsewhere".
      - Node affinity is specified as field nodeAffinity of field affinity in the PodSpec.
        ```yaml
        apiVersion: v1
        kind: Pod
        metadata:
          name: with-node-affinity
        spec:
          affinity:
            nodeAffinity:
              requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                  - key: kubernetes.io/e2e-az-name
                    operator: In
                    values:
                    - e2e-az1
                    - e2e-az2
              preferredDuringSchedulingIgnoredDuringExecution:
              - weight: 1
                preference:
                  matchExpressions:
                  - key: another-node-label-key
                    operator: In
                    values:
                    - another-node-label-value
          containers:
          - name: with-node-affinity
            image: k8s.gcr.io/pause:2.0
        ```
        - This node affinity rule says the pod can only be placed on a node with a label whose key is kubernetes.io/e2e-az-name and whose value is either e2e-az1 or e2e-az2. In addition, among nodes that meet that criteria, nodes with a label whose key is another-node-label-key and whose value is another-node-label-value should be preferred.
      - If you specify both nodeSelector and nodeAffinity, both must be satisfied for the pod to be scheduled onto a candidate node.
      - If you specify multiple nodeSelectorTerms associated with nodeAffinity types, then the pod can be scheduled onto a node if one of the nodeSelectorTerms can be satisfied.
      - If you specify multiple matchExpressions associated with nodeSelectorTerms, then the pod can be scheduled onto a node only if all matchExpressions is satisfied.
      - The weight field in preferredDuringSchedulingIgnoredDuringExecution is in the range 1-100. For each node that meets all of the scheduling requirements (resource request, RequiredDuringScheduling affinity expressions, etc.), the scheduler will compute a sum by iterating through the elements of this field and adding "weight" to the sum if the node matches the corresponding MatchExpressions. This score is then combined with the scores of other priority functions for the node. The node(s) with the highest total score are the most preferred.
    - Node affinity per scheduling profile
      - When configuring multiple scheduling profiles, you can associate a profile with a Node affinity, which is useful if a profile only applies to a specific set of Nodes. To do so, add an addedAffinity to the args of the NodeAffinity plugin in the scheduler configuration.
      - Example :
      ```yaml
      apiVersion: kubescheduler.config.k8s.io/v1beta1
      kind: KubeSchedulerConfiguration

      profiles:
        - schedulerName: default-scheduler
        - schedulerName: foo-scheduler
          pluginConfig:
            - name: NodeAffinity
              args:
                addedAffinity:
                  requiredDuringSchedulingIgnoredDuringExecution:
                    nodeSelectorTerms:
                    - matchExpressions:
                      - key: scheduler-profile
                        operator: In
                        values:
                        - foo
      ```
      - The addedAffinity is applied to all Pods that set .spec.schedulerName to foo-scheduler, in addition to the NodeAffinity specified in the PodSpec. That is, in order to match the Pod, Nodes need to satisfy addedAffinity and the Pod's .spec.NodeAffinity.
      - Since the addedAffinity is not visible to end users, its behavior might be unexpected to them. We recommend to use node labels that have clear correlation with the profile's scheduler name.
    - Inter-pod affinity and anti-affinity
      - Inter-pod affinity and anti-affinity allow you to constrain which nodes your pod is eligible to be scheduled based on labels on pods that are already running on the node rather than based on labels on nodes.
      - The rules are of the form "this pod should (or, in the case of anti-affinity, should not) run in an X if that X is already running one or more pods that meet rule Y".
      - Note: Inter-pod affinity and anti-affinity require substantial amount of processing which can slow down scheduling in large clusters significantly. We do not recommend using them in clusters larger than several hundred nodes.
      - Note: Pod anti-affinity requires nodes to be consistently labelled, in other words every node in the cluster must have an appropriate label matching topologyKey. If some or all nodes are missing the specified topologyKey label, it can lead to unintended behavior.
      - As with node affinity, there are currently two types of pod affinity and anti-affinity, called requiredDuringSchedulingIgnoredDuringExecution and preferredDuringSchedulingIgnoredDuringExecution which denote "hard" vs. "soft" requirements.
      - Example
      ```yaml
      apiVersion: v1
      kind: Pod
      metadata:
        name: with-pod-affinity
      spec:
        affinity:
          podAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                matchExpressions:
                - key: security
                  operator: In
                  values:
                  - S1
              topologyKey: topology.kubernetes.io/zone
          podAntiAffinity:
            preferredDuringSchedulingIgnoredDuringExecution:
            - weight: 100
              podAffinityTerm:
                labelSelector:
                  matchExpressions:
                  - key: security
                    operator: In
                    values:
                    - S2
                topologyKey: topology.kubernetes.io/zone
        containers:
        - name: with-pod-affinity
          image: k8s.gcr.io/pause:2.0
      ```
        - The affinity on this pod defines one pod affinity rule and one pod anti-affinity rule. In this example, the podAffinity is requiredDuringSchedulingIgnoredDuringExecution while the podAntiAffinity is preferredDuringSchedulingIgnoredDuringExecution.
        - The pod affinity rule says that the pod can be scheduled onto a node only if that node is in the same zone as at least one already-running pod that has a label with key "security" and value "S1". (More precisely, the pod is eligible to run on node N if node N has a label with key topology.kubernetes.io/zone and some value V such that there is at least one node in the cluster with key topology.kubernetes.io/zone and value V that is running a pod that has a label with key "security" and value "S1".)
        - The pod anti-affinity rule says that the pod should not be scheduled onto a node if that node is in the same zone as a pod with label having key "security" and value "S2".
      - The legal operators for pod affinity and anti-affinity are In, NotIn, Exists, DoesNotExist.
      - In principle, the topologyKey can be any legal label-key. However, for performance and security reasons, there are some constraints on topologyKey:
        1. For pod affinity, empty topologyKey is not allowed in both requiredDuringSchedulingIgnoredDuringExecution and preferredDuringSchedulingIgnoredDuringExecution.
        2. For pod anti-affinity, empty topologyKey is also not allowed in both requiredDuringSchedulingIgnoredDuringExecution and preferredDuringSchedulingIgnoredDuringExecution.
        3. For requiredDuringSchedulingIgnoredDuringExecution pod anti-affinity, the admission controller LimitPodHardAntiAffinityTopology was introduced to limit topologyKey to kubernetes.io/hostname. If you want to make it available for custom topologies, you may modify the admission controller, or disable it.
        4. Except for the above cases, the topologyKey can be any legal label-key.
      - In addition to labelSelector and topologyKey, you can optionally specify a list namespaces of namespaces which the labelSelector should match against (this goes at the same level of the definition as labelSelector and topologyKey). If omitted or empty, it defaults to the namespace of the pod where the affinity/anti-affinity definition appears.
      - All matchExpressions associated with requiredDuringSchedulingIgnoredDuringExecution affinity and anti-affinity must be satisfied for the pod to be scheduled onto a node.
    - More Practical Use-cases
      - Interpod Affinity and AntiAffinity can be even more useful when they are used with higher level collections such as ReplicaSets, StatefulSets, Deployments, etc. One can easily configure that a set of workloads should be co-located in the same defined topology, eg., the same node.
      - Always co-located in the same node
        - In a three node cluster, a web application has in-memory cache such as redis. We want the web-servers to be co-located with the cache as much as possible.
        - Here is the yaml snippet of a simple redis deployment with three replicas and selector label app=store. The deployment has PodAntiAffinity configured to ensure the scheduler does not co-locate replicas on a single node.
        ```yaml
        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: redis-cache
        spec:
          selector:
            matchLabels:
              app: store
          replicas: 3
          template:
            metadata:
              labels:
                app: store
            spec:
              affinity:
                podAntiAffinity:
                  requiredDuringSchedulingIgnoredDuringExecution:
                  - labelSelector:
                      matchExpressions:
                      - key: app
                        operator: In
                        values:
                        - store
                    topologyKey: "kubernetes.io/hostname"
              containers:
              - name: redis-server
                image: redis:3.2-alpine
        ```
        - The below yaml snippet of the webserver deployment has podAntiAffinity and podAffinity configured. This informs the scheduler that all its replicas are to be co-located with pods that have selector label app=store. This will also ensure that each web-server replica does not co-locate on a single node.
        ```yaml
        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: web-server
        spec:
          selector:
            matchLabels:
              app: web-store
          replicas: 3
          template:
            metadata:
              labels:
                app: web-store
            spec:
              affinity:
                podAntiAffinity:
                  requiredDuringSchedulingIgnoredDuringExecution:
                  - labelSelector:
                      matchExpressions:
                      - key: app
                        operator: In
                        values:
                        - web-store
                    topologyKey: "kubernetes.io/hostname"
                podAffinity:
                  requiredDuringSchedulingIgnoredDuringExecution:
                  - labelSelector:
                      matchExpressions:
                      - key: app
                        operator: In
                        values:
                        - store
                    topologyKey: "kubernetes.io/hostname"
              containers:
              - name: web-app
                image: nginx:1.16-alpine
        ```
        - If we create the above two deployments, our three node cluster should look like below :
        <table>
          <thead>
            <tr>
              <th style="text-align:center">node-1</th>
              <th style="text-align:center">node-2</th>
              <th style="text-align:center">node-3</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td style="text-align:center"><em>webserver-1</em></td>
              <td style="text-align:center"><em>webserver-2</em></td>
              <td style="text-align:center"><em>webserver-3</em></td>
            </tr>
            <tr>
              <td style="text-align:center"><em>cache-1</em></td>
              <td style="text-align:center"><em>cache-2</em></td>
              <td style="text-align:center"><em>cache-3</em></td>
            </tr>
          </tbody>
        </table>
      - Never co-located in the same node
        - The above example uses PodAntiAffinity rule with topologyKey: "kubernetes.io/hostname" to deploy the redis cluster so that no two instances are located on the same host.
  - nodeName
    - nodeName is the simplest form of node selection constraint, but due to its limitations it is typically not used.
    - nodeName is a field of PodSpec.
    - If it is non-empty, the scheduler ignores the pod and the kubelet running on the named node tries to run the pod. Thus, if nodeName is provided in the PodSpec, it takes precedence over the above methods for node selection.
    - Some of the limitations of using nodeName to select nodes are:
      - If the named node does not exist, the pod will not be run, and in some cases may be automatically deleted.
      - If the named node does not have the resources to accommodate the pod, the pod will fail and its reason will indicate why, for example OutOfmemory or OutOfcpu.
      - Node names in cloud environments are not always predictable or stable.
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
      nodeName: kube-01
    ```

Taints and Tolerations
  - Node affinity, is a property of Pods that attracts them to a set of nodes (either as a preference or a hard requirement). Taints are the opposite -- they allow a node to repel a set of pods.
  - Tolerations are applied to pods, and allow (but do not require) the pods to schedule onto nodes with matching taints.
  - Taints and tolerations work together to ensure that pods are not scheduled onto inappropriate nodes. One or more taints are applied to a node; this marks that the node should not accept any pods that do not tolerate the taints.
  - Concepts
    - You add a taint to a node using kubectl taint.
      - kubectl taint nodes node1 key1=value1:NoSchedule
        - places a taint on node node1. The taint has key key1, value value1, and taint effect NoSchedule. This means that no pod will be able to schedule onto node1 unless it has a matching toleration.
    - To remove the taint
      - kubectl taint nodes node1 key1=value1:NoSchedule-
    - You specify a toleration for a pod in the PodSpec.
      - Both of the following tolerations "match" the taint created by the kubectl taint line above :
      ```yaml
      tolerations:
      - key: "key1"
        operator: "Equal"
        value: "value1"
        effect: "NoSchedule"
      tolerations:
      - key: "key1"
        operator: "Exists"
        effect: "NoSchedule"
      ```
    - The default value for operator is Equal.
    - A toleration "matches" a taint if the keys are the same and the effects are the same, and:
      - the operator is Exists (in which case no value should be specified), or
      - the operator is Equal and the values are equal.
    - Note : There are two special cases:
      - An empty key with operator Exists matches all keys, values and effects which means this will tolerate everything.
      - An empty effect matches all effects with key key1.
    - The above example used effect of NoSchedule. Alternatively, you can use effect of PreferNoSchedule. This is a "preference" or "soft" version of NoSchedule -- the system will try to avoid placing a pod that does not tolerate the taint on the node, but it is not required. The third kind of effect is NoExecute.
    - You can put multiple taints on the same node and multiple tolerations on the same pod. The way Kubernetes processes multiple taints and tolerations is like a filter: start with all of a node's taints, then ignore the ones for which the pod has a matching toleration; the remaining un-ignored taints have the indicated effects on the pod.
    - In particular
      - if there is at least one un-ignored taint with effect NoSchedule then Kubernetes will not schedule the pod onto that node
      - if there is no un-ignored taint with effect NoSchedule but there is at least one un-ignored taint with effect PreferNoSchedule then Kubernetes will try to not schedule the pod onto the node
      - if there is at least one un-ignored taint with effect NoExecute then the pod will be evicted from the node (if it is already running on the node), and will not be scheduled onto the node (if it is not yet running on the node).
    - Normally, if a taint with effect NoExecute is added to a node, then any pods that do not tolerate the taint will be evicted immediately, and pods that do tolerate the taint will never be evicted. However, a toleration with NoExecute effect can specify an optional tolerationSeconds field that dictates how long the pod will stay bound to the node after the taint is added.
    ```yaml
    tolerations:
    - key: "key1"
      operator: "Equal"
      value: "value1"
      effect: "NoExecute"
      tolerationSeconds: 3600
    ```
      - means that if this pod is running and a matching taint is added to the node, then the pod will stay bound to the node for 3600 seconds, and then be evicted. If the taint is removed before that time, the pod will not be evicted.

  - Example Use Cases
    - Taints and tolerations are a flexible way to steer pods away from nodes or evict pods that shouldn't be running. A few of the use cases are
      - Dedicated Nodes: If you want to dedicate a set of nodes for exclusive use by a particular set of users, you can add a taint to those nodes (say, kubectl taint nodes nodename dedicated=groupName:NoSchedule) and then add a corresponding toleration to their pods (this would be done most easily by writing a custom admission controller).
      - Nodes with Special Hardware: In a cluster where a small subset of nodes have specialized hardware (for example GPUs), it is desirable to keep pods that don't need the specialized hardware off of those nodes, thus leaving room for later-arriving pods that do need the specialized hardware.
      - Taint based Evictions: A per-pod-configurable eviction behavior when there are node problems.
  - Taint based Evictions
    - The NoExecute taint effect, mentioned above, affects pods that are already running on the node as follows
      - pods that do not tolerate the taint are evicted immediately
      - pods that tolerate the taint without specifying tolerationSeconds in their toleration specification remain bound forever
      - pods that tolerate the taint with a specified tolerationSeconds remain bound for the specified amount of time
    - The node controller automatically taints a Node when certain conditions are true. The following taints are built in:
      - node.kubernetes.io/not-ready: Node is not ready. This corresponds to the NodeCondition Ready being "False".
      - node.kubernetes.io/unreachable: Node is unreachable from the node controller. This corresponds to the NodeCondition Ready being "Unknown".
      - node.kubernetes.io/out-of-disk: Node becomes out of disk.
      - node.kubernetes.io/memory-pressure: Node has memory pressure.
      - node.kubernetes.io/disk-pressure: Node has disk pressure.
      - node.kubernetes.io/network-unavailable: Node's network is unavailable.
      - node.kubernetes.io/unschedulable: Node is unschedulable.
      - node.cloudprovider.kubernetes.io/uninitialized: When the kubelet is started with "external" cloud provider, this taint is set on a node to mark it as unusable. After a controller from the cloud-controller-manager initializes this node, the kubelet removes this taint.
    - In case a node is to be evicted, the node controller or the kubelet adds relevant taints with NoExecute effect. If the fault condition returns to normal the kubelet or node controller can remove the relevant taint(s).
    -  Note: The control plane limits the rate of adding node new taints to nodes. This rate limiting manages the number of evictions that are triggered when many nodes become unreachable at once (for example: if there is a network disruption).
    - Note: Kubernetes automatically adds a toleration for node.kubernetes.io/not-ready and node.kubernetes.io/unreachable with tolerationSeconds=300, unless you, or a controller, set those tolerations explicitly.
  - Taint Nodes by Condition
    - The node lifecycle controller automatically creates taints corresponding to Node conditions with NoSchedule effect. 
