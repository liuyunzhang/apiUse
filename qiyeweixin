import requests
import json
import email
from email.header import decode_header
import email.mime
import base64

def emailDecode(_string):
    decoded_subject = ""
    for part, encoding in decode_header(_string):
        if isinstance(part, bytes):
            decoded_subject += part.decode(encoding or "utf-8")
        else:
            decoded_subject += part
    return decoded_subject

def getEmailInfo(emailId,access_token):
    """获取邮件内容"""
    url = f"""https://qyapi.weixin.qq.com/cgi-bin/exmail/app/read_mail?access_token={access_token}"""
    jsonParam = {
        "mail_id": emailId
    }
    headers = {
        "content-type": "application/json"
    }
    rep = requests.post(url=url,json=jsonParam,headers=headers)
    
    repTestJo = json.loads(rep.text)
    if repTestJo["errcode"] == 0:
        return repTestJo
    else:
        print(rep.text) 
        return

def parse():
    mailInfo = getEmailInfo("邮件id","access_token")
    mail_data = mailInfo["mail_data"]
    msg = email.message_from_string(mail_data)
   # 获取基本属性
   From = emailDecode(msg["From"])
   Subject = emailDecode(msg["Subject"])
    # 获取附件
   # 判断是否有附件
   if msg.is_multipart():
       for part in msg.get_payload():
           if part.get_content_disposition() and "attachment" in part.get_content_disposition():
                filename = emailDecode(part.get_filename())
           attachment_data = part.get_payload(decode=True)
                attachment_content_type = part.get_content_type()
                print(f"Attachment Filename:{filename}")
                print(f"Attachment Content Type:{attachment_content_type}" )
                if attachment_content_type == "application/octet-stream":
                    # 转换为 Base64 
             # base64_data =base64.b64encode(attachment_data).decode()
              # 保存为 PDF 文件
              with open(f"invoiceFileBak/{invoice_number}.pdf", "wb") as f:
                  f.write(attachment_data)
    else:
        body = msg.get_payload(decode=True)
        decoded_body = quopri.decodestring(body).decode("utf-8",errors='replace')  
        print(decoded_body)
