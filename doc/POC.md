# Розгортання ArgoCD

1. Створемо кластер (для демонстраційних цілей вистачить single node кластеру)
    
    ```bash
    k3d cluster create agro
    ```
    
2. Створемо окремий неймспейс під потреби AgroCD
    
    ```bash
    kubectl create namespace argocd
    ```
    
3. Застосуємо інсталяційний YAML-маніфест файл в рамках неймспейсу argocd, тобто проінсталюємо AgroCD
    
    ```bash
    kubectl apply -n argocd -f [https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/core-install.yaml](https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/core-install.yaml)
    ```
    

1. Перевіремо стан новостворених подів в неймспейсі argocd з оновленням інформації режимі реального часу. В данному випадку контейнери для argocd розподілені по окремим подам. 
    
    ```bash
    kubectl get po -n argocd -w
    ```
    
2. Отримаємо доступ до інтрфейсу AgroCD. 
    
    Типові варіанти: 
    
    - За допомогою сервісу з типом Load Balancer
    - Port Forwarding
    
    Скористаємось Port Forwarding
    
    ```bash
    kubectl port-forward svc/argocd-server -n argocd 8080:443
    ```
    
    Тобто для сервісу svc/argocd-server з неймспейсу agrocd налаштуємо переадресацію з локального порту 8080 на віддалений 443 в контейнері, на якому працює сервер argocd.
    
3. Отримаємо пароль
    
    ```bash
    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"|base64 -d;echo
    ```
    
    Команда отримує файл сікрету, декодує пароль і виводить його в термінал.
    
4. Отриманий пароль та логін `admin` вводимо в Web-інтерфейс ArgoCD за адресою `127.0.0.1:8080` . Погодимось з використанням самопідписаного сертифікату.