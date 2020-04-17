# Prueba concepto DevOps
# Requisitos previos
- git
- terraform
- AWS cli
- clonar el repositorio https://innersource.accenture.com/scm/demdav/demo-aws-daviplata.git

# Infraestructura como código

Los scripts o módulos de infraestructura se construyeron con terraform y tienen un apuntamiento a la nube de AWS, se encuentra en la carpeta "terraform" y están compuestos de la siguiente forma:

  -	Provider: Configuración de acceso por medio de credenciales a la cuenta de AWS, region en la cual se aprovisionara la                 infraestructura y configuración de kubernetes (kubeconfig).

  -	Variables: Archivo de variables.

  -	Data-sources.tf: Configuración de acceso a datos que ya están configurados previamente en la cuenta AWS, a los cuales se les va a     requerir para aprovisionar nuevos recursos.

  -	Eks-cluster-iam.tf: Configuración de roles necesarios para el aprovisionamiento de un clúster de kubernetes (Elastic Kubernetes       Service).

  -	Eks-cluster.tf: Configuración del clúster de kubernetes en el cual se requieren los roles creados anteriormente en el módulo           anterior.

  -	Eks-node-group-iam.tf: Configuración de los roles necesarios para el aprovisionamiento de los worker o nodos que estarán disponibles   para utilizar por el clúster (EKS), creado en el módulo anterior.

  -	Eks-node-group.tf: Configuración de el grupo de maquinas o nodos que estarán disponibles para utilizar por el clúster (EKS), en el     cual se requieren los permisos creados en el módulo anterior.

  -	Helm.tf: Despliegue de contenedores que nos facilitaran las siguientes herramientas: 
   -	Jenkins
   -	Sonarqube
   
  A las cuales se les especifica la regla de ingress (Host y servicio al que apuntará), según corresponda.
  Además, el contenedor “nginx-ingress” el cual funcionará como balanceador y actuará por medio de las reglas de ingress configuradas     anteriormente reglas de ingress.

# Ejecución de los scripts: 
Previamente a este paso debe tener instalado terraform, haber configurado el archivo de variables y helm.
Abrir la consola “cmd” o “poweshell” en el caso de Windows y ejecutar los siguientes comandos:

    git clone https://innersource.accenture.com/scm/demdav/demo-aws-daviplata.git 
    cd terraform
    terraform init
    terraform apply
    
Una vez realizado el "apply" terraform mostrara en la consola los recursos que se van a aprovisionar y posteriormente se debe confirmar con “yes”.

Una vez haya terminado, revisar la infraestructura en su consola de AWS y la configuración del cluster EKS.

Para conectarse al cluster de kuberne



