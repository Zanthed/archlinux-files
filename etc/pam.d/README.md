- [login](#login)

# [login](login)
Added the following line to enforce and require hardware U2F key before allowing password authentication:

`auth		required	pam_u2f.so noreuseok origin=pam://caligulasAquarium appid=pam://caligulasAquarium` 