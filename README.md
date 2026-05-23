# Terraform S3 Static Website Lab

Deploy a static website to AWS S3 using Terraform — locally AND via GitHub Actions.

## 🎯 What You'll Learn

- Configure AWS credentials (IAM Access Keys)
- Write Terraform configuration files
- Deploy infrastructure with `terraform apply`
- Automate deployment with GitHub Actions
- Clean up with `terraform destroy`

## 📋 Prerequisites

| Tool | Install Command | Verify |
|------|----------------|--------|
| AWS CLI | `choco install awscli` | `aws --version` |
| Terraform | `choco install terraform` | `terraform --version` |
| Git | `choco install git` | `git --version` |

## 🔑 Step 1: Create AWS Access Keys

1. Go to [AWS Console](https://console.aws.amazon.com/) → IAM → Users
2. Click your username → **Security credentials** tab
3. Click **Create access key** → Choose **CLI**
4. **Copy both keys immediately!** (Secret is shown only once)

## 🔧 Step 2: Configure AWS CLI

```bash
aws configure
```

Enter:
- Access Key ID: `AKIA...`
- Secret Access Key: `wJal...`
- Region: `us-east-1`
- Output: `json`

Test it:
```bash
aws sts get-caller-identity
```

## 🚀 Step 3: Deploy Locally

```bash
# Clone this repo
git clone <your-repo-url>
cd lab-s3-website

# Initialize Terraform
terraform init

# Preview what will be created
terraform plan -var="bucket_name=YOUR-UNIQUE-NAME"

# Deploy!
terraform apply -var="bucket_name=YOUR-UNIQUE-NAME"
```

After success, Terraform shows your website URL:
```
website_url = "http://YOUR-NAME.s3-website-us-east-1.amazonaws.com"
```

Open it in your browser! 🎉

## 🤖 Step 4: Automate with GitHub Actions

1. Push this repo to GitHub
2. Go to repo **Settings → Secrets and variables → Actions**
3. Add these secrets:

| Secret Name | Value |
|-------------|-------|
| `AWS_ACCESS_KEY_ID` | Your access key |
| `AWS_SECRET_ACCESS_KEY` | Your secret key |
| `AWS_REGION` | `us-east-1` |
| `TF_VAR_bucket_name` | Your unique bucket name |

4. Push to `main` → GitHub Actions auto-deploys!

## 🗑️ Step 5: Clean Up (IMPORTANT!)

**Locally:**
```bash
terraform destroy -var="bucket_name=YOUR-UNIQUE-NAME"
```

**Via GitHub Actions:**
- Go to Actions tab → "Destroy Infrastructure" → Run workflow

⚠️ **Always destroy when done to avoid charges!**

## 📁 Project Structure

```
lab-s3-website/
├── .github/workflows/
│   ├── deploy.yml          # Auto-deploy on push to main
│   └── destroy.yml         # Manual destroy workflow
├── website/
│   ├── index.html          # Your website (edit this!)
│   ├── error.html          # 404 page
│   └── style.css           # Styling
├── providers.tf            # AWS provider configuration
├── main.tf                 # S3 bucket + website config
├── variables.tf            # Input variables
├── outputs.tf              # Output values (website URL)
├── terraform.tfvars.example # Example variables
├── .gitignore              # Terraform ignores
└── README.md               # This file
```

## ⚠️ Security Reminders

- ❌ NEVER put access keys in `.tf` files
- ❌ NEVER commit `.tfvars` files with secrets
- ❌ NEVER share your Secret Access Key
- ✅ Use `aws configure` for local development
- ✅ Use GitHub Secrets for CI/CD
- ✅ Delete access keys you don't use

## 🔗 Resources

- [Terraform AWS Provider Docs](https://registry.terraform.io/providers/hashicorp/aws/latest)
- [S3 Static Website Hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)
- [GitHub Actions for Terraform](https://github.com/hashicorp/setup-terraform)
- [Cybr.com Lab Reference](https://cybr.com/hands-on-labs/lab/create-a-static-s3-website-using-terraform/)
