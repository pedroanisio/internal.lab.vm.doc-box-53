## ISCP-60 version

```iscp60
&v1.0.1;core=1.0,doc=1.2,infra=1.1;
<@protocol{name:"ISCP-60", version:"1.0.1", domain:"infrastructure_documentation"}>

<@context{domain:"homelab", subject:"documentation_vm"}>
  @define{id:VM, value:"doc-box-53"}
  @define{id:DOMAIN, value:"internal.lab.vm"}
  @define{id:IP, value:"10.10.10.53"}
  @define{id:OS, value:"Debian 12.10"}
  @define{id:PURPOSE, value:"documentation_hub"}
  @define{id:CREATION_DATE, value:"2025-03"}

  <@entity{type:"virtual_machine"}>
    @identity{hostname:$VM, domain:$DOMAIN, ip:$IP}
    
    @specification{
      hypervisor:"Proxmox VE 8.3.5",
      host:"pve-box-2",
      vcpu:2,
      ram:"1024 GiB",
      storage:"64 GiB SSD",
      os:$OS,
      pool:"mngmt"
    }
    
    @purpose{
      primary:$PURPOSE,
      description:"centralized documentation hub for entire homelab infrastructure",
      justification:[
        "single source of truth for all homelab infrastructure",
        "easy access to server configurations and maintenance procedures",
        "knowledge preservation for future reference and troubleshooting"
      ]
    }
    
    @lifecycle{
      created:$CREATION_DATE,
      retirement:"none",
      maintenance_window:"Sundays, 2 AM - 4 AM"
    }
    
    @dependencies{
      depends_on:["network infrastructure", "DNS server"],
      depended_on_by:["all homelab services"]
    }
    
    <@services>
      @service{
        name:"MkDocs Material",
        type:"documentation",
        description:"documentation framework that renders Markdown as documentation sites",
        container:"squidfunk/mkdocs-material",
        ports:["8001-8010:8000"]
      }
      
      @service{
        name:"Traefik",
        type:"proxy",
        description:"reverse proxy for centralized routing",
        container:"traefik:latest",
        ports:["80:80", "443:443"]
      }
      
      @service{
        name:"Watchtower",
        type:"maintenance",
        description:"Docker image updater",
        container:"containrrr/watchtower"
      }
    </@services>
    
    <@network_config>
      @config{
        type:"static",
        ip:$IP,
        exposed_ports:["80", "443", "8001-8010"]
      }
    </@network_config>
    
    <@file_structure>
      @directory{
        path:"/srv/docs",
        owner:"pals",
        contains:[
          "docker-compose.yml",
          {
            path:"repos",
            contains:[
              "arc4d3",
              "internal.lab.vm.doc-box-53"
            ]
          }
        ]
      }
    </@file_structure>
    
    <@maintenance>
      <@backup>
        @policy{
          schedule:"Daily @ 11:37 UTC",
          retention:"7 days",
          location:"PBS storage on pbs-box-73",
          type:"Full VM backup",
          verification:"Weekly automated"
        }
        
        @restoration{
          vm_procedure:[
            "Access Proxmox web interface",
            "Navigate to Backup section",
            "Select appropriate backup",
            "Click Restore and select target node",
            "Verify network settings before restoring"
          ],
          content_procedure:[
            "Clone repository from GitLab",
            "Or restore from exported archive",
            "Restart documentation container"
          ]
        }
      </@backup>
      
      <@updates>
        @schedule{
          security:"Weekly (automated)",
          system:"Monthly (manual)",
          os_version:"As needed (planned 2 weeks in advance)"
        }
        
        @procedure{
          manual:[
            "sudo apt update",
            "sudo apt upgrade -y",
            "check if reboot required"
          ],
          docker:"Automatic via Watchtower (30-minute intervals)",
          documentation:"Git-based content updates"
        }
        
        @rollback{
          system:[
            "Use ppa-purge for package issues",
            "Restore from VM backup for major issues"
          ],
          docker:[
            "Roll back to specific image version",
            "Edit docker-compose.yml",
            "Restart containers"
          ],
          documentation:"Git revert to previous commit"
        }
      </@updates>
    </@maintenance>
    
    <@documentation_structure>
      @template{
        format:"hierarchical with reversed hierarchy naming",
        pattern:"internal.lab.vm.{vm-name}",
        example:"internal.lab.vm.doc-box-53"
      }
      
      @file_structure{
        root:"/srv/docs/repos/internal.lab.vm.{vm-name}/",
        components:[
          "docs/index.md",
          "docs/reference/",
          "docs/services/",
          "docs/maintenance/",
          "mkdocs.yml"
        ]
      }
      
      @guidelines{
        style:[
          "Be concise and use clear language",
          "Use consistent formatting and Markdown",
          "Include all essential information",
          "Keep documentation updated"
        ],
        required_sections:[
          "Purpose and business justification",
          "Server specifications",
          "Network configuration",
          "Services running",
          "Maintenance procedures",
          "Dependency relationships"
        ]
      }
    </@documentation_structure>
    
    <@access>
      @endpoint{
        direct:"http://10.10.10.53:8000",
        proxy:"https://docs.local.arc4d3.io",
        internal_port:"8000",
        external_port:"443"
      }
    </@access>
  </@entity>
</@context>

&meta{author:"ISCP-60 Conversion", date:"2025-03-25", version:"1.0", purpose:"machine_interpretable_documentation"}
```