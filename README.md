# Applied Technology Skills - Labs

![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazonwebservices&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-777BB4?style=flat-square&logo=php&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)

A collection of hands-on lab exercises for the Applied Technology Skills (ATS) module. These labs guide students through cloud infrastructure, web analytics, WordPress integration, and command-line fundamentals using AWS and related technologies.

## Labs

| Lab | Description |
|-----|-------------|
| [Bash and Nano Fundamentals](bash-nano-lab.md) | Introduction to basic Bash commands, file management, and the Nano text editor |
| [AWS CLI Introduction](aws-cli-lab.md) | Create and manage EC2 instances, security groups, key pairs, and Elastic IPs using the AWS CLI via CloudShell |
| [HTTPS Certificates via AWS](https-certs-via-aws.md) | Configure an Application Load Balancer with SSL/TLS certificates using ACM and Route 53 |
| [Matomo Analytics Server](analytics-server.md) | Install and configure Matomo open-source web analytics on a LAMP stack with WordPress integration |
| [WordPress AI Integration](wp-ai-chat.md) | Integrate AI chatbot capabilities into WordPress using the AI Engine plugin and Google Gemini API |
| [WordPress API Integration](wp-api.md) | Fetch and display external API data on a WordPress page using PHP shortcodes |
| [How to Get a Domain Name](how-to-get-a-domain-name.md) | Quick reference for obtaining domain names through GitHub Student Pack, AWS, and registrars |

## Prerequisites

- An [AWS Academy](https://aws.amazon.com/training/awsacademy/) or personal AWS account
- A web browser with access to [AWS CloudShell](https://aws.amazon.com/cloudshell/)
- Basic familiarity with the command line
- A WordPress installation (for WordPress-related labs)

## Getting Started

1. **Clone the repository:**
   ```bash
   git clone https://github.com/danielcregg/ats-labs.git
   cd ats-labs
   ```

2. **Choose a lab** from the table above and follow the step-by-step instructions in the corresponding Markdown file.

3. **Clean up AWS resources** after completing each lab to avoid unexpected charges.

## Tech Stack

- **AWS** -- EC2, CloudShell, Route 53, ACM, Elastic Load Balancing
- **WordPress** -- Plugins, shortcodes, WP-CLI
- **Matomo** -- Open-source web analytics
- **LAMP** -- Linux, Apache, MySQL, PHP
- **Google Gemini API** -- AI chatbot integration

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
