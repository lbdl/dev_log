## 2FA

Seems to now be enabled for Jack and others, ??

either way we see 2fa errors in CI according to the docs its session related and likely to do with the dev certs which we dont  set to `readonly:true` so we might be recreating them?

anyway seems the session key mentioned [FASTLANE_SESSION](https://docs.fastlane.tools/getting-started/ios/authentication/) works

Likely thins will need to be reset regularly and we may not be able to automate this

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgwMDQxMDk0M119
-->