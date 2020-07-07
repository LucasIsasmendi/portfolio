# Firebase

## Keys
Go to settings -> Service Account -> Generate New private key
for serviceAccountKey

## Deploy
### Multiple environments
`firebase use --add`
`firebase deploy -P staging`

## Analytics
https://www.simoahava.com/analytics/getting-started-with-google-analytics-app-web/

## Knowledge Base

### EVENTS

#### subscribe/unsubscribe

### Database
#### id
https://firebase.googleblog.com/2015/02/the-2120-ways-to-ensure-unique_68.html

What's in a Push ID?

A push ID contains 120 bits of information. The first 48 bits are a timestamp, which both reduces the chance of collision and allows consecutively created push IDs to sort chronologically. The timestamp is followed by 72 bits of randomness, which ensures that even two people creating push IDs at the exact same millisecond are extremely unlikely to generate identical IDs

## Tips & Tricks
### Connect 1 FB proyect - 2 DBs
https://stackblitz.com/edit/angular-or2ehb?file=src%2Fapp%2Fapp.component.ts

https://github.com/angular/angularfire/issues/1026
