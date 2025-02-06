# Laboratoire S3

## Installation et configuration du CLI

* [Installer le client git](https://git-scm.com/)
  * Git bash vous embarque de nombreuses commandes Linux utiles pour s'entrainer avec S3 (cat, grep, touch, ls)
* [Installation du CLI - v2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
* [Configuration du CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-quickstart.html#getting-started-quickstart-existing)
  * Les clés vous ont été partagées par Teams
* [Gestion des profiles](https://docs.aws.amazon.com/cli/v1/userguide/cli-configure-files.html#cli-configure-files-format-profile)
  * Vous devez créer un profile du nom de votre équipe et l'utiliser pour toute les futures commandes
  ```
  aws s3 <la commande> --profile devopsteam<xx>
  ```

## IAM Policy

Voici la "policy" qui vous a été assignée:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:ListAllMyBuckets",
            "Resource": "arn:aws:s3:::*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject",
            ],
            "Resource": "arn:aws:s3:::devopsteam<XX>-i346/*" //XX -> devopsteam number
        }
    ]
}
```

## Exploiter un S3

Attention:
* Vous devez utiliser la v2 du CLI
* Le client offre soit la commande s3, soit s3api. En priorité vous devez essayer "s3". Voici une [documentation AWS traitant de ce sujet](https://aws.amazon.com/blogs/developer/leveraging-the-s3-and-s3api-commands/).
* Si l'action que vous désirez réaliser sur le bucket est refusée pour des raisons de droits, trouvez une autre possibilité de réaliser cela.

### Créer un bucket

* [AWS Official Doc - Create Bucket](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/mb.html#examples)

* Le bucket existe-t-il ?

```bash
aws s3 ls --profile devopsteam<XX>-i346 | grep "devopsteam*"
```

```
[OUTPUT]
2025-01-27 22:23:30 devopsteam01-i346
[...]
2025-02-03 19:32:33 devopsteam10-i346
```

* Créer un bucket (via un compte admin)

```bash
aws s3 mb s3://devopsteam<XX>-i346 --region eu-central-1 --profile s3-admin
```

```
[OUTPUT]
make_bucket: devopsteam99-i346
```


### Uploader un fichier

* [AWS Official Doc - Upload]()

* [Vérifier l'état du bucket avant votre commande]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

### Uploader un répertoire

* [AWS Official Doc - Upload a folder]()

* [Vérifier l'état du bucket avant votre commande]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

### Lister le contenu d'un "repertoire"

* [AWS Official Doc - List bucket content]()

* [Vérifier l'état du bucket avant votre commande]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

### Synchroniser un répertoire local de sa machine avec un bucket

* [AWS Official Doc - Synchronize local folder]()

* [Vérifier l'état du bucket avant votre commande]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

### Publier un fichier présent sur un bucket en générant un lien (url) temporaire

* [AWS Official Doc - Generate temporary link]()

* [Vérifier l'état du bucket avant votre commande]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

### Supprimer un objet

* [AWS Official Doc - Remove object]()

* [Vérifier l'état du bucket avant votre commande]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

### Vider un "repertoire"

* [AWS Official Doc - Empty "folder"]()

* [Vérifier l'état du bucket avant votre commande]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

### Extraire uniquement les metadonnées d'un objet

* [AWS Official Doc - Get Object Metadata only()

* [Vérifier l'état du bucket avant votre commande]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

### Vider le bucket

* [AWS Official Doc - Empty bucket]()

* [Vérifier l'état du bucket avant votre commande]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
//TODO
```

```
[OUTPUT]
//TODO
```

---

## Questions d'analyse

Consigne : répondre en utilisant des sources officielles et en vous appuyant dessus pour répondre.

### Pourquoi est-il déconseillé de détruire un bucket S3 selon AWS ?

* [Sources AWS]

[Votre réponse]

### Quelle est la différence entre un Bucket S3 et Glacier ?

* [Sources AWS]

[Votre réponse]

### Reprenez l'IAM "Policy" et expliquez ce que vous pouvez en déduire au niveau des droits qui vous sont alloués

Consigne : Reprenez la "policy" et documenter chaque ligne
