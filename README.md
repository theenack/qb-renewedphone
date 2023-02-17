# qb-renewedphone
Fix for phone images broke


1. Create an imgur account, go to https://api.imgur.com/oauth2/addclient, create an application (for callback URL use: https://www.getpostman.com/oauth2/callback) and get the Client-ID.
2. Put the Client ID in screenshot-basic\ui\src\ui\src\main.ts - line 204. - Do not remove Client-ID part, only change YOURCLIENTID with your Client-ID
https://imgur.com/a/9fh89ib
NOTE: Changes to screenshot-basic need to be done before first building (running) it or delete dist and node_modules folders.
3. Go to qb-phone's config.lua and change the webhook option to this: Config.Webhook = 'imgur'
4. Go to qb-phone\client\main.lua and change the lines from 616 to 629 with this:
QBCore.Functions.TriggerCallback("qb-phone:server:GetWebhook",function(hook)
    QBCore.Functions.Notify('Touching up photo...', 'primary')
    exports['screenshot-basic']:requestScreenshotUpload(tostring(hook), "files[]", function(uploadData)
        DestroyMobilePhone()
        CellCamActivate(false, false)
        TriggerServerEvent('qb-phone:server:addImageToGallery', uploadData.data.link)
        Wait(400)
        TriggerServerEvent('qb-phone:server:getImageFromGallery')
        cb(json.encode(uploadData.data.link))
        QBCore.Functions.Notify('Photo saved!', "success")
        OpenPhone()
        exports['screenshot-basic']:requestScreenshotUpload(tostring("PUTDISCORDWEBHOOKHERE"), "files[]", function(uploadData) end)
    end)
end)
