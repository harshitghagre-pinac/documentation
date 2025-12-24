# Alerts setup 
* First simple thing grafana provide default alert using email if need to change to another contact point we can simply configure it by going to `Home -> Alerting -> Contact Points -> Create Contact Points`.
* Create new contact point provide name, integration, message template.

# Alert Rules 
* It's an condition when the alert notification will fires.
* For creating alert rule go ` Home -> Alerting -> Manage alert rules`.
* Create new alert rule provide the name, define query and alert condition ( which query we want alert's ), add folder where all the rules will be store.
* Set evalution behaviour add the evaluation group & interval ( for evaluating the query in particualr time spawn ) eg. pending period, keep firing for , also configure no data and error handling.
* Now configure notifications set the contact point eg. gmail webhook , email, etc.