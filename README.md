# Unmasking Secrets in GitLab CI/CD Job Logs

## Description:

This proof of concept demonstrates a potential vulnerability within GitLab's CI/CD pipelines regarding the handling of secret variables. Despite GitLab's built-in secret masking feature, which is intended to protect sensitive data from being exposed in job logs, we show that this mechanism can be bypassed under certain conditions.

In the initial stage of the pipeline, the `export` command is used to display all environment variables in the shell, including secret variables. These variables are then redirected to a file named `secrets.txt` using `export > secrets.txt`. This operation results in all of the environment variables, including any secrets, being stored in plaintext within the file. The `secrets.txt` file is then declared as an artifact of the job, which means it can be passed to subsequent jobs in the pipeline and can also be downloaded after the pipeline execution. It's crucial to recognize that this operation exposes the secret variables in an unprotected format, thereby posing a significant security risk. This stage of the pipeline demonstrates that, if mismanaged, secrets can be accidentally exposed or stored insecurely, underlining the importance of handling sensitive data appropriately within CI/CD pipelines.

In our PoC, we use a GitLab CI/CD pipeline that contains a script which dissects a secret into individual characters. By inserting a space between each character, we are able to output the entirety of the secret in plaintext within the job logs. Although the secret is masked when echoed directly, our method reveals the secret by exploiting the masking feature's inability to recognize and mask the separated characters of the secret.

This PoC underscores the importance of being aware of potential security loopholes, even within systems that have security measures in place. Despite GitLab's effort to protect secrets, users must be cautious in their handling of sensitive data within CI/CD pipelines to prevent inadvertent exposure.

**Disclaimer**: This PoC is intended for educational purposes and should only be used in controlled and secure environments. Exposing sensitive data in logs can pose a significant security risk and is generally not recommended in production environments. Always ensure appropriate measures are taken to protect sensitive information in your CI/CD pipelines.




