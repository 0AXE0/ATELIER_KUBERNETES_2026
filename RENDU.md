# Rendu — Atelier Kubernetes

> Complétez ce fichier au fil de l'atelier, puis faites un commit et un push (voir la section « Rendu » du README).

**Nom / Prénom :**
**Groupe :**

---

## Exercice 1 — Cluster

1. Combien de nœuds affiche `kubectl get nodes` ? Quel est le rôle de chacun ?
   > Le cluster Kubernetes contient 3 nœuds qui sont
   > - k3d-lab-server-0 : nœud control-plane (master), qui gère le cluster.
   > - k3d-lab-agent-0 : premier nœud worker.
   > - k3d-lab-agent-1 : deuxième nœud worker.

2. Que montre `docker ps` ? Qu'est-ce qu'un « nœud » dans k3d, concrètement ?
   > docker ps affiche les 4 conteneurs Docker en cours d’exécution, 1 control-plane, 2 workers et 1 load balancer.
   
   >Dans k3d, un nœud Kubernetes est concrètement un conteneur Docker qui joue le rôle d’un serveur dans le cluster.

## Exercice 2 — Premier déploiement

3. Quels objets Kubernetes ont été créés par `kubectl create deployment` ? (indice : `kubectl get all`)
   > Un Deployment, qui crée automatiquement un ReplicaSet, qui crée à son tour un Pod exécutant nginx.                                  Le Service a été créé séparément avec kubectl expose.

4. Vous avez supprimé le pod `web-...` : que s'est-il passé, et pourquoi ?
   > Le pod a été supprimé, puis Kubernetes en a automatiquement créé un nouveau avec un nom différent web-68d995574f-r6nmx → web-68d995574f-82jsc.                                          C'est grâce au ReplicaSet, qui maintient le nombre de pods demandé.

## Exercice 3 — Modifier un conteneur en direct

5. Après suppression du pod, votre message « Bienvenue... » a-t-il survécu ? Pourquoi ?
   > Non, le message a disparu, car le nouveau pod est recréé à partir de l'image nginx d'origine. Les modifications faites directement dans le conteneur sont temporaires.
6. Pourquoi le `port-forward` a-t-il cessé de fonctionner après la suppression du pod ?
   > Le port-forward était connecté à l'ancien pod. Lors de sa suppression, la connexion a été interrompue. Il faut donc relancer le tunnel vers le nouveau pod.

## Exercice 4 — Déclaratif

7. Quelle différence entre `kubectl create` et `kubectl apply` ?
   > kubectl create crée une nouvelle ressource Kubernetes, tandis que kubectl apply permet de créer ou de mettre à jour une ressource à partir d'un fichier YAML.
8. À quoi sert le `selector` d'un Service ? Que se passe-t-il s'il ne correspond à aucun pod ?
   > Le selector permet au Service d'identifier les pods grâce à leurs labels. Si aucun pod ne correspond, le Service n'a aucune cible vers laquelle envoyer le trafic.

## Exercice 5 — Scaling & mises à jour

9. Sur quels nœuds se trouvent vos 3 réplicas ? Collez la sortie de `kubectl get pods -l app=site -o wide`.
>  ```
NAME                     READY  STATUS   IP         NODE
site-6b4966d78d-4drzz    1/1    Running  10.42.0.6  k3d-lab-agent-1
site-6b4966d78d-528mb    1/1    Running  10.42.1.7  k3d-lab-server-0
site-6b4966d78d-7l68r    1/1    Running  10.42.2.8  k3d-lab-agent-0
>   ```

10. Avec l'image `nginx:9.99`, quel statut avait le pod ? Quelle commande vous a donné la cause ?
    > Le pod avait le statut ErrImagePull, puis ImagePullBackOff, car l'image nginx:9.99 est introuvable.
11. Pendant la panne, le site restait-il accessible ? Pourquoi ?
    > Normalement oui, car Kubernetes conserve les 3 anciens pods fonctionnels pendant le rolling update. La version défectueuse ne remplace pas les pods disponibles.

## Exercice 6 — Portainer & Mario

12. URL publique de votre Mario :
    > https://animated-halibut-p4vg6pg4g9qh76x4-8585.app.github.dev/
13. Retrouvez le Deployment `mario` en ligne de commande. Portainer et `kubectl` parlent-ils au même composant ? Lequel ?
    > Oui, Portainer et kubectl communiquent tous les deux avec l'API Server de Kubernetes pour gérer les ressources du même cluster.                                    Portainer utilise une interface graphique, tandis que kubectl utilise la ligne de commande.

## Bonus (facultatif)

14. Bonus réalisés et remarques :
    > Yep done it, but it's 17/18 :(

---

**Score obtenu avec `./scripts/check.sh` :** ... / ...
