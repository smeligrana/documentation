Gestione de diritti sui bucket
=============================================
In minio i diritti che un utente ha su un bucket vengono gestiti tramite le policy, di seguito un esempio con creazione di due bucket e due utenti che possono operare in modalità full sul proprio bucket. 

Le operazioni vengono svolte tramite la console web completa 
  * Creo i 2 bucket user1bucket e user2bucket 
  * Creo le policy come da file policy.txt 
  * Creo gli utenti user1 e user 2 e ad ognuno associo la policy corrispondente. 

'''
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
'''
