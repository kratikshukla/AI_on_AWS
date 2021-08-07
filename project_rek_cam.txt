import cv2
import boto3  


cap = cv2.VideoCapture(0)
myphoto = "kratik.jpg"
ret , photo = cap.read()
cv2.imwrite( myphoto , photo)
cap.release()

region = "ap-south-1"
bucket = "ai-aws-workshop123"

upimage = "kratik.jpg"

s3 = boto3.resource('s3')

s3.Bucket(bucket).upload_file(myphoto , upimage)

rek = boto3.client('rekognition' , region )

response = rek.detect_labels(
Image={
          'S3Object': {
              'Bucket': bucket,
              'Name': upimage,
          }
      },
      MaxLabels=10,
      MinConfidence=90
)

for i in range(5):
    print ( response ['Labels'][i]['Name'])