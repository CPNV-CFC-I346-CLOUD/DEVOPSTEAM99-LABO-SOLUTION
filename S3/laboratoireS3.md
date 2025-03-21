# Laboratoire S3

## Installation et configuration du CLI

* [Installer le client git](https://git-scm.com/)
  * [Git bash](https://www.atlassian.com/git/tutorials/git-bash) vous permettra de pouvoir expérimenter des commandes Linux, tout en étant sur un environnement Windows.
* [Installation du CLI - v2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
* [Configuration du CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-quickstart.html#getting-started-quickstart-existing)
  * Les clés vous ont été partagées par oneDrive
  * Pour faciliter leur mise en place, utiliser la commande "configure" du CLI d'AWS
  ```
  aws configure
  ```
  
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

Note : elle n'offre pas la totalité des droits dont vous aurez besoin pour réaliser le laboratoire. Ceci est volontaire. Vous devrez néanmoins trouver les bonnes commandes. Puis en fin de laboratoire les droits manquants vous serons assignés afin de valider votre documentation.

## Exploiter un S3

Attention:
* Vous devez utiliser la v2 du CLI. En recherchant sur la toile, vous aurez fréquemment des documentations relatifs à la v1 qui reste très populaire.
* Le client offre soit la commande s3, soit s3api. En priorité vous devez essayer "s3".

### Créer un bucket (ceci a été réalisé par votre enseignant)

* [AWS Official Doc - Create Bucket](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/mb.html#examples)

* Le bucket existe-t-il ?

```bash
aws s3 ls --profile devopsteam99-i346 | grep "devopsteam*"
```

```
[OUTPUT]
2025-01-27 22:23:30 devopsteam01-i346
[...]
2025-02-03 19:32:33 devopsteam10-i346
```

* Créer un bucket (via un compte admin)

```bash
aws s3 mb s3://devopsteam99-i346 ^
--region eu-central-1 ^
--profile s3-admin
```

```
[OUTPUT]
make_bucket: devopsteam99-i346
```

### Prerequisites

* (Optionnel) installer wget pour Windows

[Wget Setup for Windows](https://eternallybored.org/misc/wget/)

* Créer la structure de fichiers utilisée pour faire les tests.

```
touch uploadFile.txt
mkdir folderToUpload
touch folderTopUpload/bob.txt folderToUpload/script.sql
wget https://calaero.edu/wp-content/uploads/2018/05/Airplane-Transponder.jpg
```

```
└───data
    │   fileToUpload.txt
    │   Airplane-Transponder.jpg
    └───folderToUpload
            bob.txt
            script.sql
```

### Uploader un fichier

* [AWS Official Doc - Upload](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/cp.html)

* [Vérifier l'état du bucket avant votre commande]

```bash
aws s3 ls s3://devopsteam99-i346 ^
--profile devopsteam99-i346
```

```
[OUTPUT]
<empty>
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
aws s3 cp fileToUpload.txt s3://devopsteam99-i346/fileToUpload.txt ^
--profile devopsteam99-i346
```

```
[OUTPUT]
upload: .\fileToUpload.txt to s3://devopsteam99-i346/fileToUpload.txt
```

### Uploader un répertoire

* [AWS Official Doc - Upload](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/cp.html)

* [Vérifier que le répertoire n'est pas déjà présent]

```bash
aws s3 ls s3://devopsteam99-i346 ^
--profile devopsteam99-i346
```

```
[OUTPUT]
2025-02-23 09:09:10          0 fileToUpload.txt
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
aws s3 cp folderToUpload s3://devopsteam99-i346/folderToUpload ^
--recursive ^
--profile devopsteam99-i346
```

```
[OUTPUT]
upload: folderToUpload\bob.txt to s3://devopsteam99-i346/folderToUpload/bob.txt
upload: folderToUpload\script.sql to s3://devopsteam99-i346/folderToUpload/script.sql
```

### Lister le contenu d'un "repertoire"

* [AWS Official Doc - List bucket content](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/ls.html)

* [La commande à réaliser pour effectuer l'action demandée]

```bash
aws s3 ls s3://devopsteam99-i346 ^
--recursive ^
--profile devopsteam99-i346
```

```
[OUTPUT]
2025-02-23 09:09:10          0 fileToUpload.txt
2025-02-23 09:10:21          0 folderToUpload/bob.txt
2025-02-23 09:10:21          0 folderToUpload/script.sql
```

### Synchroniser un répertoire local de sa machine avec un bucket

* [AWS Official Doc - Synchronize local folder](https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html)

* [Vérifier l'état du bucket avant votre commande]

```bash
aws s3 ls s3://devopsteam99-i346 ^
--recursive ^
--profile devopsteam99-i346
```

```
[OUTPUT]
2025-02-23 09:09:10          0 fileToUpload.txt
2025-02-23 09:10:21          0 folderToUpload/bob.txt
2025-02-23 09:10:21          0 folderToUpload/script.sql
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
aws s3 sync . s3://devopsteam99-i346/ ^
--profile devopsteam99-i346
```

```
[OUTPUT]
upload: .\Airplane-Transponder.jpg to s3://devopsteam99-i346/Airplane-Transponder.jpg
```

### Publier un fichier présent sur un bucket en générant un lien (url) temporaire

* [AWS Official Doc - Generate temporary link](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/presign.html)

* [Vérifier si l'objet à publier est présent sur le bucket]

```bash
aws s3api head-object ^
--bucket devopsteam99-i346 ^
--key Airplane-Transponder.jpg ^
--profile devopsteam99-i346
```

```
[OUTPUT]
{
    "AcceptRanges": "bytes",
    "LastModified": "2025-02-23T08:12:59+00:00",
    "ContentLength": 17010,
    "ETag": "\"d0194f28e675232be0018b34d760fb92\"",
    "ContentType": "image/jpeg",
    "ServerSideEncryption": "AES256",
    "Metadata": {}
}
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
aws s3 presign s3://devopsteam99-i346/Airplane-Transponder.jpg ^
--expires-in 60 ^
--region eu-central-1 ^
--profile devopsteam99-i346
```

```
[OUTPUT]
https://devopsteam99-i346.s3.eu-central-1.amazonaws.com/Airplane-Transponder.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIA2KFJKL4O6EAOCHBR%2F20250223%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Date=20250223T081917Z&X-Amz-Expires=60&X-Amz-SignedHeaders=host&X-Amz-Signature=674d4a08094ad410c871d26aeb8a9166d267d798c6c633fb2b07ba7ee734e068
```

### Supprimer un objet

* [AWS Official Doc - Remove object](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/rm.html)

* [Vérifier si l'objet à supprimer est présent sur le bucket]

```bash
aws s3api head-object ^
--bucket devopsteam99-i346 ^
--key fileToUpload.txt ^
--profile devopsteam99-i346
```

```
[OUTPUT]

{
    "AcceptRanges": "bytes",
    "LastModified": "2025-02-23T17:00:50+00:00",
    "ContentLength": 0,
    "ETag": "\"d41d8cd98f00b204e9800998ecf8427e\"",
    "ContentType": "text/plain",
    "ServerSideEncryption": "AES256",
    "Metadata": {}
}
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
aws s3 rm s3://devopsteam99-i346/fileToUpload.txt ^
--profile devopsteam99-i346
```

```
[OUTPUT]
delete: s3://devopsteam99-i346/fileToUpload.txt
```

### Vider un "repertoire"

* [AWS Official Doc - Empty "folder"](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/rm.html)

* [Vérifier le contenu du répertoire à vider]

```bash
aws s3 ls s3://devopsteam99-i346/folderToUpload ^
--recursive ^
--profile devopsteam99-i346
```

```
[OUTPUT]
2025-02-23 09:10:21          0 folderToUpload/bob.txt
2025-02-23 09:10:21          0 folderToUpload/script.sql
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
aws s3 rm s3://devopsteam99-i346/folderToUpload ^
--recursive ^
--profile devopsteam99-i346
```

```
[OUTPUT]
delete: s3://devopsteam99-i346/folderToUpload/script.sql
delete: s3://devopsteam99-i346/folderToUpload/bob.txt
```

### Extraire uniquement les metadonnées d'un objet

* [AWS Official Doc - Get Object Metadata only](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3api/get-object-attributes.html)

* [Vérifier la présence de l'objet dont nous voulons extraire les metadonnées]

```bash
aws s3api head-object ^
--bucket devopsteam99-i346 ^
--key Airplane-Transponder.jpg ^
--profile devopsteam99-i346
```

```
[OUTPUT]
{
    "AcceptRanges": "bytes",
    "LastModified": "2025-02-23T08:32:02+00:00",
    "ContentLength": 17010,
    "ETag": "\"d0194f28e675232be0018b34d760fb92\"",
    "ContentType": "image/jpeg",
    "ServerSideEncryption": "AES256",
    "Metadata": {}
}
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
aws s3api get-object-attributes ^
--bucket devopsteam99-i346 ^
--key Airplane-Transponder.jpg ^
--object-attributes ObjectSize ^
--profile devopsteam99-i346
```

```
[OUTPUT]
{
{
    "LastModified": "2025-02-23T08:32:02+00:00",
    "ObjectSize": 17010
}
```

### Vider le bucket

* [AWS Official Doc - Empty bucket](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/rm.html)

* Resynchroniser votre bucket avec votre répertoire de test pour que l'action de "vidage" du Bucket soit mieux observable

* Lister le contenu du répertoire

```bash
aws s3 ls s3://devopsteam99-i346/ ^
--recursive ^
--profile devopsteam99-i346
```

```
[OUTPUT]
2025-02-23 09:32:02      17010 Airplane-Transponder.jpg
2025-02-23 09:09:10          0 fileToUpload.txt
2025-02-23 09:32:01          0 folderToUpload/bob.txt
2025-02-23 09:32:02          0 folderToUpload/script.sql
```

* [La commande à réaliser pour effectuer l'action demandée]

```bash
aws s3 rm s3://devopsteam99-i346/ ^
--recursive ^
--profile devopsteam99-i346
```

```
[OUTPUT]
delete: s3://devopsteam99-i346/folderToUpload/bob.txt
delete: s3://devopsteam99-i346/folderToUpload/script.sql
delete: s3://devopsteam99-i346/fileToUpload.txt
delete: s3://devopsteam99-i346/Airplane-Transponder.jpg
```

---

## Questions d'analyse

Consigne : répondre en utilisant des sources officielles et en vous appuyant dessus pour répondre.

### Pourquoi est-il déconseillé de détruire un bucket S3 selon AWS ?

* [Sources AWS](https://docs.aws.amazon.com/AmazonS3/latest/userguide/delete-bucket.html)

AWS déconseille de supprimer un bucket. Le nom étant unique est impactant les DNS, une suppression/recréation (par exemple lors de tests automatique) risque de rendre l'état des DNS instables.
En supprimant un Bucket, vous permettez à un autre utilisateur de le créer et vous risquez ainsi de ne plus pouvoir l'utiliser ce nom.
Le coût d'un Bucket vide est nul. Autrement dit si vous désirez utiliser ce Bucket dans un avenir proche, laissez-le en place.

### Quelle est la différence entre un Bucket S3 et Glacier ?

* [Sources AWS](https://aws.amazon.com/s3/storage-classes/glacier/)

Glacier est un produit utilisant les mêmes concepts d'"Object Storage" que S3. La différence majeure est la nécessité d'accéder aux données stockées. En effet Glacier est prévu pour de l'archivage. Le temps de récupération peut être de plusieurs heures (max 12 heures).
Cette lenteur qui n'est pas acceptable pour l'hébergement de fichier dont nous avons régulièrement besoin et cependant très intéressante pour des archives. Le coût de stockage devient dérisoire sur Glacier.

Rapide comparatif
|Produit|Temps de récupération|Coût ($/Gb|
|:--|:--|:--|
|S3 Standard|Immédiat|Entre 0.0021-0.0023|
|Glacier Deep Archive|Jusqu'à 12 heures|0.00099|

[Source](https://aws.amazon.com/s3/pricing/)

### Reprenez l'IAM "Policy" et expliquez ce que vous pouvez en déduire au niveau des droits qui vous sont alloués

Consigne : Reprenez la "policy" et documentez les lignes essentielles.

```json
{
    "Version": "2012-10-17",                                                          //version de la policy
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:ListAllMyBuckets",                                          //autorisation à lister tous les buckets de notre compte
            "Resource": "arn:aws:s3:::*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",                                                      //autorisation de Put, Get, Delete uniquement sur le bucket d'un nom spécifique
                "s3:GetObject",
                "s3:DeleteObject",
            ],
            "Resource": "arn:aws:s3:::devopsteam<XX>-i346/*" //XX -> devopsteam number //restriction de nom de bucket
        }
    ]
}
```

#### En plus

##### Pourquoi avoir opté pour s3api pour valider si l'object exist ?

En me documentant pour trouver une méthode pour faire un "doesExist", le forum d'AWS m'a redirigé vers cette solution qui semble être favorisée.

* En version 1 du cli, il y a une commande [object-exists](https://docs.aws.amazon.com/cli/latest/reference/s3api/wait/object-exists.html).
* En version 2 du cli, c'est [head-object](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3api/wait/object-exists.html) qui est favorisée par AWS.

Dans les deux versions, c'est s3api qui utilisé.

Pour une approche avec le cli et une commande s3 native, les solutions sont multiples et non standardisées. C'est la raison pour laquelle j'ai opté pour le s3api.

* Voir cette échange sur [StackOverflow](https://stackoverflow.com/questions/41871948/aws-s3-how-to-check-if-a-file-exists-in-a-bucket-using-bash)

