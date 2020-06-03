+++
# Experience widget.
widget = "experience"  # See https://sourcethemes.com/academic/docs/page-builder/
headless = true  # This file represents a page section.
active = true  # Activate this widget? true/false
weight = 40  # Order that this section will appear.

title = "Experience"
subtitle = "My work experience"

# Date format for experience
#   Refer to https://sourcethemes.com/academic/docs/customization/#date-format
date_format = "Jan 2006"

# Experiences.
#   Add/remove as many `[[experience]]` blocks below as you like.
#   Required fields are `title`, `company`, and `date_start`.
#   Leave `date_end` empty if it's your current employer.
#   Begin/end multi-line descriptions with 3 quotes `"""`.

[[experience]]
  title = "Systems Architect"
  company = "Questback GmbH"
  company_url = "https://www.questback.com/"
  location = "Cologne, Germany"
  date_start = "2019-04-01"
  date_end = ""
  description = """
  
  Responsibilities include:
  
  * Automating infrastructure setup with terraform (AWS infrastructure and network, Rancher Management Cluster, downstream Kubernetes Clusters, Azure Authentication, etc.)
  * Create CI/CD pipeline to deploy applications to Kubernetes clusters
  * Use packer to create AWS AMI for applicaion deployment outside of Kubernetes Clusters
  * Work close with development and product management on requirements for modernization of legacy application
  * Work remote with a consultancy from India for ~ 10 months (experience the pros and cons of remote work)

  """

[[experience]]
  title = "Systems Engineer"
  company = "Questback GmbH"
  company_url = "https://www.questback.com/"
  location = "Cologne, Germany"
  date_start = "2018-02-01"
  date_end = "2019-03-31"
  description = """

  Responsibilities include:

  * Elaborate a way to run Kubernetes on AWS (Rancher2 with rke)
  * Lift legacy application to Kubernetes and AWS from VM based datacenter
  * Create helm chart for legacy application
  * Load test internal application, tune metrics and change design in the helm-chart accordingly
  * Implement tools and software around the K8s ecosystem: helm, chartmuseum, nginx-ingress controller, cert-manager, Prometheus, Grafana, EFK, cloudwatch-exporter, Prometheus Adapter for K8s Metrics, etc.

  """

[[experience]]
  title = "IT Service Management/Operations"
  company = "Modell Aachen GmbH"
  company_url = "https://www.modell-aachen.de/"
  location = "Aachen, Germany"
  date_start = "2017-02-01"
  date_end = "2018-01-31"
  description = """

  Responsibilities include:

  * Automating infrastructure deployment with puppet and ansible
  * Implementing processes for internal IT needs
  * Implementing KANBAN based ticket system for internal Office IT
  * Optimizing internal IT processes
  * Supervising student trainee

  """

[[experience]]
  title = "Student Worker"
  company = "Modell Aachen GmbH"
  company_url = "https://www.modell-aachen.de/"
  location = "Aachen, Germany"
  date_start = "2015-04-01"
  date_end = "2016-01-31"
  description = """

  Responsibilities include:

  * Consulting customers how to implement processes and management documentation with the product (Q.wiki) developed by Modell Aachen GmbH (On sight training)
  * Creating content for Q.wiki demo system
  * Automating Laptop Deployment
  * Implementing company-wide password management software

  """
+++
