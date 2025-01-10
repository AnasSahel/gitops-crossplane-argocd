# Tutorial

## Installation du provider

Pour installer le Provider `Azure`, il suffit de lancer la commande suivante

```bash
kubectl -f provider-azure.yaml
```

## Création de l'`Azure Service Principal`

L'`Azure Service Principal` est nécessaire pour que `Crossplane` puisse dialoguer avec `Azure`
Il faut lancer la commande suivante et stocker son résultat dans un fichier `json`

```bash
az ad sp create-for-rbac \
--skd-auth \
--role Owner \
--scopes /subscriptions/<subscription_id>
```

- `--sdk-auth` est `deprecated`
- `Owner` est un rôle très large. Il faudra trouver un autre moyen plus sécurisé

## Création du secret `Kubernetes` pour le provider `Azure`

Si le résultat de la commande précédente est enregistré dans `azure-credentials.json`, il faut l'enregistrer en tant que secret dans `Kubernetes` en lançant la commande suivante

```bash
kubectl create secret \
generic azure-secret \
-n crossplane-system \
--from-file=creds=./azure-credentials.json
```

## Création de la `ConfigProvider`

Maintenant que le `Provider` est installé, le secret l'est aussi, il faudra lié les deux

```bash
kubectl apply -f azure-config-provider.yaml
```
