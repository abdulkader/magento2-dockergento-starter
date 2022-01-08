# Magento 2 Setup Boilerplate with Dockergento

1. elasticsearch:7.12.1
2. mysql:5.7
3. nginx:1.13
4. php:7.4-fpm-1

## Installation

1. Follow dockergento installation setup for your system [here](https://github.com/ModestCoders/magento2-dockergento)
2. Clone this repository
3. Add `magento-local.com` to your host file entry or customize the domain here `config/dockergento/nginx/conf/default.conf`
4. You need have your local docker running for setting up the magento docker environment

```
...
... other host file entries
127.0.0.1 magento-local.com
```

4. Run `dockergento start` from the cloned project root folder
5. If you visit `http://magento-local.com/` in your browser, you should see magento welcome screen before the setup

### Setting Up Magento for your local

1. We need to disable the Magento Elasticsearch modules before installation and will enable once installation is complete.

Run below command from the project root

```
dockergento magento module:disable {Magento_Elasticsearch,Magento_InventoryElasticsearch,Magento_Elasticsearch6,Magento_Elasticsearch7}
```

2. Run below command to setup the local magento. You can modify the parameter values with your information

```
dockergento magento setup:install --admin-firstname=Admin --admin-lastname=User --admin-email=admin@mydomain.com --admin-user=master --admin-password=master123 --base-url=http://magento-local.com/ --backend-frontname=admin --db-host=db --db-name=magento --db-user=magento --db-password=magento --use-rewrites=1 --session-save=db --language=en_US --currency=AED --timezone=Asia/Dubai --elasticsearch-host=elasticsearch --elasticsearch-port=9200
```

3. We can disable all non required module which comes with default installation. You can update or remove the module list from below if you require any of the module.

```
dockergento magento module:disable {Magento_AdobeStockImageAdminUi,Magento_AdminAnalytics,Magento_AdobeIms,Magento_AdobeImsApi,Magento_AdobeStockAdminUi,Magento_AdobeStockAssetApi,Magento_AdobeStockClient,Magento_AdobeStockClientApi,Magento_AdobeStockImage,Magento_AdobeStockImageApi,Magento_Fedex,Dotdigitalgroup_Email,Dotdigitalgroup_Chat,Dotdigitalgroup_ChatGraphQl,Dotdigitalgroup_EmailGraphQl,Dotdigitalgroup_Sms,Klarna_Core,Klarna_Ordermanagement,Klarna_Kp,Klarna_Onsitemessaging,Klarna_KpGraphQl,Magento_ReCaptchaAdminUi,Magento_ReCaptchaCheckout,Magento_ReCaptchaContact,Magento_ReCaptchaCustomer,Magento_ReCaptchaFrontendUi,Magento_ReCaptchaMigration,Magento_ReCaptchaNewsletter,Magento_ReCaptchaPaypal,Magento_ReCaptchaReview,Magento_ReCaptchaSendFriend,Magento_ReCaptchaStorePickup,Magento_ReCaptchaUi,Magento_ReCaptchaUser,Magento_ReCaptchaValidation,Magento_ReCaptchaValidationApi,Magento_ReCaptchaVersion2Checkbox,Magento_ReCaptchaVersion2Invisible,Magento_ReCaptchaVersion3Invisible,Magento_ReCaptchaWebapiApi,Magento_ReCaptchaWebapiGraphQl,Magento_ReCaptchaWebapiRest,Magento_ReCaptchaWebapiUi,Magento_TwoFactorAuth,Magento_AdobeStockAsset,Magento_Dhl,Magento_GoogleOptimizer,Magento_Ups,Temando_ShippingRemover,Yotpo_Yotpo,PayPal_Braintree,PayPal_BraintreeGraphQl} --clear-static-content
```

4. We can enable the elasticsearch modules now

```
dockergento magento module:enable {Magento_Elasticsearch,Magento_InventoryElasticsearch,Magento_Elasticsearch6,Magento_Elasticsearch7}
```

5. Run `dockergento magento setup:di:compile` for compiling the classes

### Setting Up Magento sample data

1. You need to have a magento account on [Magento Marketplace](https://marketplace.magento.com/) for getting the API key for installation

2. Run `dockergento magento sampledata:deploy`

3. Once packages have been downloaded, run `dockergento magento setup:upgrade`

4. If you get error `Could not validate a connection to Elasticsearch. No alive nodes found in your cluster` Please run, `dockergento start` again and try re running the upgrade
