# Disable attachment downloads
To make attachments not available to download. Follow the instructions below to disable attachment downloads.

Attachments can currently be downloaded in two separate ways:
* Viewing the attachments for a page
* Viewing all the attachments for a Space (Browse > Attachments)

These customizations will disable attachment downloads for all users, including administrators.

## Disable attachments for **a whole Space**
Comment out the following line in file listattachmentsforspace.vm:
```
<td><a name="$!generalUtil.urlEncode($!attachment.content.displayTitle)-attachment-$!generalUtil.urlEncode($!attachment.fileName)">#parse ("/pages/includes/attachment_icon.vm")</a> <a href="$req.contextPath$!attachment.downloadPathWithoutVersion">$generalUtil.shortenString($attachment.fileName, 50)</a></td>
```
and replace it with either of the following two code blocks:
1. Disabling downloading for *all* attachments
    ```
    <td><a name="$!generalUtil.urlEncode($!attachment.content.displayTitle)-attachment-$!generalUtil.urlEncode($!attachment.fileName)">#parse ("/pages/includes/attachment_icon.vm")</a> $generalUtil.shortenString($attachment.fileName, 50)</td>
    ```
1. Disabling downloading for *specific file types* (e.g. docx and jpg)
    ```
    #set($disabledDownloads = ['docx', 'jpg'])
    #set($disabled = false)
    #set($attachmentExtension = $attachment.fileExtension)
    <tr id="attachment_$!attachment.id">
        #foreach($doNotDownload in $disabledDownloads)
            #if($attachmentExtension == $doNotDownload)
                #set($disabled = true)
                #break
            #end
        #end
        #if(!$disabled)
            <td><a name="$!generalUtil.urlEncode($!attachment.content.displayTitle)-attachment-$!generalUtil.urlEncode($!attachment.fileName)">#parse ("/pages/includes/attachment_icon.vm")</a> <a href="$req.contextPath$!attachment.downloadPathWithoutVersion">$generalUtil.shortenString($attachment.fileName, 50)</a></td>
        #else
            <td><a name="$!generalUtil.urlEncode($!attachment.content.displayTitle)-attachment-$!generalUtil.urlEncode($!attachment.fileName)">#parse ("/pages/includes/attachment_icon.vm")</a> $generalUtil.shortenString($attachment.fileName, 50)</td>
        #end
    ```
## Disable attachments for **a specific page**
Comment out the following line in file attachments-table.vm:
```
<a class="filename" href="$generalUtil.htmlEncode("${req.contextPath}${attachment.downloadPathWithoutVersion}")" title="$generalUtil.htmlEncodeAndReplaceSpaces($attachment.fileName)" >$generalUtil.htmlEncode($generalUtil.shortenString($attachment.fileName, 35))</a>
```
and replace it with either of the following two code blocks:
1. Disabling downloading for *all* attachments
    ```
    $generalUtil.htmlEncode($generalUtil.shortenString($attachment.fileName, 35))
    ```
1. Disabling downloading for *specific file types* (e.g. doc and png)
    ```
    #set($disabledDownloads = ['doc', 'png'])
    #set($disabled = false)
    #set($attachmentExtension = $attachment.fileExtension)
    #foreach($doNotDownload in $disabledDownloads)
        #if($attachmentExtension == $doNotDownload)
            #set($disabled = true)
            #break
        #end
    #end
    #if(!$disabled) <a class="filename" href="$generalUtil.htmlEncode("${req.contextPath}${attachment.downloadPathWithoutVersion}")" title="$generalUtil.htmlEncodeAndReplaceSpaces($attachment.fileName)" >$generalUtil.htmlEncode($generalUtil.shortenString($attachment.fileName, 35))</a>
    #else $generalUtil.htmlEncode($generalUtil.shortenString($attachment.fileName, 35))
    #end
    ```
## Remove the 'Download All' button
### Remove the button on **all spaces**
1. Click the Setup button, then General Configuration
1. Click Stylesheet, then the Edit button
1. Add the following to the Global Stylesheet
    ```css
    a.download-all-link {
        display: none !important;
    }
    a#download-all-link {
        display: none !important;
    }
    a.content-metadata-attachments {
        display: none !important;
    }
    a#content-metadata-attachments {
        display: none !important;
    }
    ```
1. Save
### Remove the button on **certain space**
1. Go to the space and choose *Space Tools* > *Look and Feel* from the bottom of the sidebar
1. Choose *Stylesheet* then *Edit*.
1. Paste the above CSS into the text field.
1. Save

## More
1. Comment out the following line in file viewattachments.vm:
```
<a id="download-all-link" href="$req.contextPath/pages/downloadallattachments.action?pageId=$pageId" title="$action.getText('download.all.desc')">$action.getText('download.all')</a>
```
[Reference](https://confluence.atlassian.com/confkb/how-to-disable-attachment-downloads-215484007.html)
