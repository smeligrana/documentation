Gestione de diritti sui bucket
=============================================
In minio i diritti che un utente ha su un bucket vengono gestiti tramite le policy, di seguito un esempio con creazione di due bucket e due utenti che possono operare in modalità full sul proprio bucket. 

Le operazioni vengono svolte tramite la console web completa 
  * Creo i 2 bucket user1bucket e user2bucket 
  * Creo le policy come da file policy.txt 
  * Creo gli utenti user1 e user 2 e ad ognuno associo la policy corrispondente. 

Questa policy fonisce i diritti sul bucket user1bucket (fare la stessa cosa per user2bucket)
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "s3:PutBucketPolicy",
        "s3:GetBucketPolicy",
        "s3:DeleteBucketPolicy",
        "s3:ListAllMyBuckets",
        "s3:ListBucket"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:s3:::user1bucket"
      ],
      "Sid": ""
    },
    {
      "Action": [
        "s3:AbortMultipartUpload",
        "s3:DeleteObject",
        "s3:GetObject",
        "s3:ListMultipartUploadParts",
        "s3:PutObject"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:s3:::user1bucket/*"
      ],
      "Sid": ""
    }
  ]
}
```

Esempio di docker-compose.yaml con minio e console web completa
```
services:
  minio:
    image: quay.io/minio/minio
    container_name: minio-server
    ports:
      - "9000:9000" # API Port
      - "9001:9001" # Web Console Port
    environment:
      MINIO_ROOT_USER: minioadmin
      MINIO_ROOT_PASSWORD: minioadmin
    volumes:
      - minio_data:/data
    command: server /data --console-address ":9001"    
    
  opens3-console:
    image: opens3/console:latest
    ports:
      - "9090:9090"
    environment:
      CONSOLE_MINIO_SERVER: "http://minio:9000" # Nome del servizio minio nel compose
      CONSOLE_PBKDF_PASSPHRASE: "una-tua-chiave-segreta"
      CONSOLE_PBKDF_SALT: "un-tuo-salt-casuale"

volumes:
  minio_data:
```
