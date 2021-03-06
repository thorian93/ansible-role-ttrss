# Ansible Role: TTRSS

This role installs TinyTinyRSS on Debian and Ubuntu servers.

[![Ansible Role: TTRSS](https://img.shields.io/ansible/role/55147?style=flat-square)](https://galaxy.ansible.com/thorian93/ttrss)
[![Ansible Role: TTRSS](https://img.shields.io/ansible/quality/55147?style=flat-square)](https://galaxy.ansible.com/thorian93/ttrss)
[![Ansible Role: TTRSS](https://img.shields.io/ansible/role/d/55147?style=flat-square)](https://galaxy.ansible.com/thorian93/ttrss)

## Known issues

None.

## Requirements

No special requirements; note that this role requires root access, so either run it in a playbook with a global `become: yes`, or invoke the role in your playbook like:

    - hosts: foobar
      roles:
        - role: thorian93.ansible_role_ttrss
          become: yes

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    ttrss_version: "18.0.7"

Define the TTRSS version you want to install.

    ttrss_create_self_signed_cert: true
    ttrss_self_signed_cert_subj: "/C=DE/ST=FOO/L=BAR/O=Org/CN={{ ttrss_external_url }}"
    ttrss_self_signed_certificate_key: "/etc/{{ apache2_http_name }}/ssl/ttrss.key"
    ttrss_self_signed_certificate: "/etc/{{ apache2_http_name }}/ssl/ttrss.crt"

Configure self signed certificates to your liking.

    ttrss_custom_cert: false
    ttrss_custom_cert_file: /etc/{{ apache2_http_name }}/ssl/ttrss.crt
    ttrss_custom_cert_key: /etc/{{ apache2_http_name }}/ssl/ttrss.key

If you want to use your own certificate you can define that here.

    ttrss_certificate_key: "{{ certbot_cert_path }}/privkey.pem"
    ttrss_certificate: "{{ certbot_cert_path }}/cert.pem"
    ttrss_certificate_chain: "{{ certbot_cert_path }}/fullchain.pem"

If `ttrss_create_self_signed_cert` and `ttrss_custom_cert` are set to false, my `ansible-role-certbot` will be used to acquire certificates.

    ttrss_db_system: "mysql"
    ttrss_db_name: "ttrss"

Configure the database for TTRSS. Currently available is only MySQL/MariaDB.

  ttrss_enable_opt_prerequisites: true

This installs some optional software which is useful for TTRSS.

    ttrss_turn_enable: 'false'
    ttrss_turn_ip: " {{ ansible_default_ipv4.address }}"
    ttrss_turn_port: 3478
    ttrss_turn_realm: "{{ ttrss_external_url }}"
    # Set this in your inventory. By default this role will generate a new secret on every run until this variable is set.
    # ttrss_turn_secret:

Enable and configure setup of a TURN server for TTRSS Talk on your server. For further information see the [TTRSS documentation](https://ttrss-talk.readthedocs.io/en/latest/TURN). Keep an eye on the `ttrss_turn_secret` variable!

    ttrss_backup: false
    ttrss_backup_path: "/tmp"

Configure Backups for TTRSS.

    ttrss_web_dir: "/var/www/ttrss"

Define the webroot of TTRSS.

    ttrss_data_dir: "/var/www/ttrss/data"

Define the data directory. It is recommended to put this outside the web root.

    ttrss_php_options:
      - line: "post_max_size = 4G"
        regexp: "^post_max_size ="
      - line: "upload_max_filesize = 4G"
        regexp: "^upload_max_filesize ="
      - line: "open_basedir ='{{ ttrss_web_dir }}:{{ ttrss_data_dir }}:/tmp:/dev/urandom'"
        regexp: "^open_basedir ="

Define PHP options for TTRSS. The defaults given here are necessary for TTRSS to work properly.

    ttrss_enabled_apps:
      - files

List the apps which should be enabled.

## Dependencies

  - [thorian93.ansible-role-apache2](https://galaxy.ansible.com/thorian93/apache2)
  - [thorian93.ansible-role-php](https://galaxy.ansible.com/thorian93/ansible_role_ttrss)
  - [thorian93.ansible-role-certbot](https://galaxy.ansible.com/thorian93/certbot) - when no custom or self signed certificate is used
  - [thorian93.ansible-role-mysql](https://galaxy.ansible.com/thorian93/mysql)

## OS Compatibility

This role ensures that it is not used against unsupported or untested operating systems by checking, if the right distribution name and major version number are present in a dedicated variable named like `<role-name>_stable_os`. You can find the variable in the role's default variable file at `defaults/main.yml`:

    role_stable_os:
      - Debian 10
      - Ubuntu 18
      - CentOS 7
      - Fedora 30

If the combination of distribution and major version number do not match the target system, the role will fail. To allow the role to work add the distribution name and major version name to that variable and you are good to go. But please test the new combination first!

Kudos to [HarryHarcourt](https://github.com/HarryHarcourt) for this idea!

## Example Playbook

    ---
    - name: "Run role."
      hosts: all
      become: yes
      roles:
        - thorian93.ansible_role_ttrss

## Contributing

Please feel free to open issues if you find any bugs, problems or if you see room for improvement. Also feel free to contact me anytime if you want to ask or discuss something.

## Disclaimer

This role is provided AS IS and I can and will not guarantee that the role works as intended, nor can I be accountable for any damage or misconfiguration done by this role. Study the role thoroughly before using it.

## License

MIT

## Author Information

This role was created in 2020 by [Thorian93](http://thorian93.de/).
