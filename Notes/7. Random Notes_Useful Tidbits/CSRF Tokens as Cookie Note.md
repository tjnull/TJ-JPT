# CSRF Tokens as Cookie Note

Sometimes Cookies are automatically sent by browser, so if the CSRF token is being automatically sent, if a victim user clicks a CSRF POC, then that request might include the CSRF token, thus making its main purpose mute. 