# AsciiArtify. Proof of Concept

> Підготуємо та розгорнемо GitOps-систему ArgoCD на Kubernetes.

1. Створемо кластер (для демонстраційних цілей вистачить single node кластеру)

   ```bash
   k3d cluster create agro
   ```
2. Створемо окремий неймспейс під потреби ArgoCD

   ```bash
   kubectl create namespace argocd
   ```
3. Для зучності встановимо поточний контекст - `argocd`

   ```bash
   kubectl config set-context --current --namespace=argocd
   ```
4. Застосуємо інсталяційний YAML-маніфест файл в рамках неймспейсу `argocd`, тобто проінсталюємо ArgoCD

   ```bash
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/core-install.yaml
   ```
5. Перевіремо стан новостворених подів в неймспейсі `argocd` з оновленням інформації режимі реального часу. В данному випадку контейнери для `argocd` розподілені по окремим подам.

   ```bash
   kubectl get po -n argocd -w
   ```
   У результаті відобразиться близько восьми подів/контейнерів.
6. Отримаємо доступ до інтерфейсу ArgoCD.

   Типові варіанти:

   - За допомогою сервісу з типом Load Balancer
   - Port Forwarding

   Скористаємось Port Forwarding

   ```bash
   kubectl port-forward svc/argocd-server -n argocd 8080:443&
   ```
   Тобто для сервісу svc/argocd-server з неймспейсу agrocd налаштуємо переадресацію у фоні з локального порту 8080 на віддалений 443 в контейнері, на якому працює веб-сервер ArgoCD.
7. Отримаємо пароль

   ```bash
   kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"|base64 -d;echo
   ```
   Команда отримує файл сікрету, декодує пароль і виводить його в термінал.
8. Отриманий пароль та логін `admin` вводимо в Web-інтерфейс ArgoCD за адресою `127.0.0.1:8080` . Погодимось з використанням самопідписаного сертифікату. Для Codespace потрібно додатково [зробити](https://docs.github.com/en/codespaces/developing-in-a-codespace/forwarding-ports-in-your-codespace#using-https-forwarding) додаткові дії по прокиданню https назовні.
