# Аутентификация

## Active Directory: период действия старого пароля <a href="#active-directory-old-password-lifetime" id="active-directory-old-password-lifetime"></a>

При использовании Active Directory в качестве внешней системы аутентификации, в течение определенного времени после смены пароля пользователь может выполнить успешный вход в систему с использованием старого пароля.

Данное поведение определяется настройками сервера LDAP. Брокер HOSTVM VDI не кэширует пароли пользователей и не управляет этим параметром.

Дополнительные сведения:

1. Настройка Windows Server:\
   [https://learn.microsoft.com/en-us/troubleshoot/windows-server/windows-security/new-setting-modifies-ntlm-network-authentication](https://learn.microsoft.com/en-us/troubleshoot/windows-server/windows-security/new-setting-modifies-ntlm-network-authentication)
2. Настройка Samba сервера в роли Active Directory Domain Controller (параметр `old password allowed period`):\
   [https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html)

