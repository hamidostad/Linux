# Path in CentOs: /etc/vsftpd/vsftpd.conf && Path in Ubuntu: /etc/vsftpd.conf
# uncomment below rows
ascii_upload_enable=YES 
ascii_download_enable=YES 
ftpd_banner=Welcome to blah FTP service
# add below lines
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
use_localtime=YES
