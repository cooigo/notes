- 文档 

    <https://developers.google.com/web/fundamentals/engage-and-retain/push-notifications/?hl=zh-cn>

- 使用教程

    * 样例 <https://tests.peter.sh/notification-generator/> 

    * 推送
        
        ```javascript
        //main.js

        'use strict';

        (function () {
            

        const applicationServerPublicKey = '';

        let isSubscribed = false;
        let swRegistration = null;

        function urlB64ToUint8Array(base64String) {
            const padding = '='.repeat((4 - base64String.length % 4) % 4);
            const base64 = (base64String + padding)
                .replace(/\-/g, '+')
                .replace(/_/g, '/');

            const rawData = window.atob(base64);
            const outputArray = new Uint8Array(rawData.length);

            for (let i = 0; i < rawData.length; ++i) {
                outputArray[i] = rawData.charCodeAt(i);
            }
            return outputArray;
        }

        //初始化
        //window.addEventListener("load", function () {
            if ('serviceWorker' in navigator) {
                navigator.serviceWorker.register("/Content/push-notifications/sw.js?v=1")
                    .then(function (swRegistration) {

                        const applicationServerKey = urlB64ToUint8Array(applicationServerPublicKey);
                        swRegistration.pushManager.subscribe({
                            userVisibleOnly: true,
                            applicationServerKey: applicationServerKey
                        })
                            .then(function (subscription) {

                                //发送订阅的服务端
                                sendSubscriptionToServer(subscription);

                            })
                            .catch(function (err) {
                                console.log('Failed to subscribe the user: ', err);
                            });

                    }); 
            }

        //});

        function sendSubscriptionToServer(subscription) {
            // TODO: Send subscription to application server
            if (subscription) {
                const subs = JSON.stringify(subscription);
                console.log(subs);

                $.ajax({
                    type: "POST",
                    data: subs,
                    url: "/api/app/send-subscription",
                    contentType: "application/json",
                    success: function () {
                        localStorage && localStorage.setItem('sentToServer', 1);
                    }
                });
            } 
        }

        })();
        ```
        

    * 通知

        ```javascript
        //sw.js

        'use strict';

        self.addEventListener('push', function (event) {
            console.log('Received a push message', event);
            const payload = event.data.json();

            const title = payload.title;
            const body = payload.body;

            event.waitUntil(
                self.registration.showNotification(title, {
                    body: body,
                    icon: payload.icon || '/content/push-notifications/images/icon.png',
                    badge: payload.badge || '/content/push-notifications/images/badge.png',
                    image: payload.image,
                    data: payload.data
                })
            );
        });

        self.addEventListener('notificationclick', function (event) {
            const payload = event.notification.data;
            console.log('On notification click: ', event.notification.data);
            event.notification.close();

            const loadfun = function () {
                if (payload && payload.url) {
                    clients.openWindow(payload.url);
                }
            }
            const promiseChain = loadfun();
            event.waitUntil(promiseChain);
        });

        ```
