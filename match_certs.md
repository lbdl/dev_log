## fastlane match with multiple identifiers

when calling `bundle exec fastlane setup_match_maryland` and the team has several identifiers, i.e. GameAccount Network Plc - KPS4TFKB6D for example where we have 
```
com.gameaccount.livesocial.slots.GANotificationService

com.gan.winstar.slots
```

amongst others if we use a`readonly:false` then `match` gets it wrong tries to create new certs and hits the limit for the account.

Therefore we need to use one repo that has the correct certs. We then push this up to the match repo as a different branch e.g using `Winstar`
```
git push origin Winstar:Maryland
```
and then just delete the profiles and let match recreate the profiles for us.

<!--stackedit_data:
eyJoaXN0b3J5IjpbNzgzMjcwODMxXX0=
-->