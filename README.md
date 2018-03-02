# app-ms-dynamics-nav2017-client

Install Application Microsoft Dynamics NAV 2017 Client on a Windows Endpoint.

## Requirements

- Ansible control server with access through WINRM
- NAV Client setup files zipped with config file 
- Windows 7 x86/amd64 or higher (Endpoint)

## Variable(s)

Al variables can be modified in the file **apps-ms-dynamics-nav2017-client/var/main.yml**.
The 64bit version is provisioned by default with 32bit settings. In case of a 64bit package
you van override these settings.

1. Generate a config file (see nav documentation) and put it in the install directory from the client.
2. Zip the install directory.
3. Upload the file to a webserver.
4. Modify the role to your settings.
5. Add the role to your playbook.

Example var/main.yml:
```
  # --- Common settings ---
  # Title of the package
  pkg_name              : "Microsoft Dynamics NAV2017 Client"

  # Path to local package
  pkg_local_path        : "{{ ansible_env.ProgramFiles }}/Ansible/temp/"


  # --- Specifick (x86) package settings ---
  pkg_remote_url        : http://nl-bel-cmd01/resource/microsoft/dynamics/nav/2017/NAV2017.zip
  pkg_name_zip          : NAV2017.zip
  pkg_product_id        : DynamicsNav100
  pkg_installer         : '{{ pkg_local_path }}/nav2017/nl/setup.exe'
  pkg_arguments         : /quiet /config "{{ pkg_local_path }}/NAV2017/NL/NAV2017-VGG-CLIENT.xml"


  # --- Specific (amd64) package settings ---

  # Uncomment the line which key to check. (Wow6432Node = 32bit)
  # pkg_reg_uninstall_path_amd64 : HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\
  pkg_reg_uninstall_path_amd64 : HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\

  pkg_remote_url_amd64   : "{{ pkg_remote_url }}"
  pkg_name_zip_amd64     : "{{ pkg_name_zip }}"
  pkg_product_id_amd64   : "{{ pkg_product_id }}"
  pkg_installer_amd64    : "{{ pkg_installer }}"
  pkg_arguments_amd64    : "{{ pkg_arguments }}"
```
