# atos-tAsk

after create the cluster and connect omn it .


-kubectl create ns jenkins
-kubectl create ns sonarqube
-kubectl create ns nexus
-kubectl create ns app-dev
-kubectl create ns app-tst
-kubectl create ns app-prd

for check:

kubectl get ns


Done!


>install jenkins



>install sonarqube
helm repo add sonarqube https://SonarSource.github.io/helm-chart-sonarqube
helm repo update
helm upgrade --install -n sonarqube sonarqube-lts sonarqube/sonarqube-lts
change service to loadbalancer

>install nexus
 helm repo add sonatype https://sonatype.github.io/helm3-charts/
  myvalues.yaml >>
   nexus:
 env:
   # minimum recommended memory settings for a small, person instance from
   # https://help.sonatype.com/repomanager3/product-information/system-requirements
   - name: INSTALL4J_ADD_VM_PARAMS
     value: |-
       -Xms2703M -Xmx2703M
       -XX:+UnlockExperimentalVMOptions -XX:ActiveProcessorCount=4
       -XX:+UseCGroupMemoryLimitForHeap
       -Djava.util.prefs.userRoot=/nexus-data/javaprefs
       -Dnexus.licenseFile=/etc/nexus-license/license.lic
       -Dnexus.datastore.enabled=true
       -Dnexus.datastore.nexus.jdbcUrl=jdbc:postgresql://postgresql-host:5432/nexusdb
       -Dnexus.datastore.nexus.username=nexus
       -Dnexus.datastore.nexus.password=nexus123
   - name: NEXUS_SECURITY_RANDOMPASSWORD
     value: "true"
# # To use an additional secret, set enable to true and add data
ingress:
  enabled: true
  ingressClassName: nginx
  annotations:
    nginx.ingress.kubernetes.io/proxy-body-size: "0"
  hostPath: /
  hostRepo: nx340.minikube.mydomain
secret:
  enabled: true
  mountPath: /etc/nexus-license/
  readOnly: true
  data:
    license.lic : 'cylwwtYx6Fjh7o4k34Ih3KMhWlu1TvWP...'
 <<
 
 helm install nx340 -n nexus -f myvalues.yaml sonatype/nexus-repository-manager -n nexus
 
change cluster ip to LoadBalancer 
docker tag myimage  <ip _nexus> /myimage
docker login nexus.example.com
docker push ip_nexus/myimage

_________________________________________________________________________________________________________
