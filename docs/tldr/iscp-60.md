## ISCP-60 version

```iscp60

&v1.0.1;core=1.0,doc=1.2,infra=1.1,config=1.3;
<@protocol{name:"ISCP-60", version:"1.0.1", domain:"infrastructure_documentation"}>

<@context{domain:"homelab", subject:"documentation_vm"}>
  @define{id:VM, value:"doc-box-53"}
  @define{id:DOMAIN, value:"internal.lab.vm"}
  @define{id:IP, value:"10.10.10.53"}
  @define{id:OS, value:"Debian 12.10"}
  @define{id:PURPOSE, value:"documentation_hub"}
  @define{id:CREATION_DATE, value:"2025-03"}
  @define{id:CONTACT, value:"admin@internal.lab.vm"}

  <@entity{type:"virtual_machine"}>
    @identity{hostname:$VM, domain:$DOMAIN, ip:$IP}
    
    @specification{
      hypervisor:"Proxmox VE 8.3.5",
      host:"pve-box-2",
      vcpu:2,
      ram:"1024 GiB",
      storage:"64 GiB SSD",
      os:$OS,
      pool:"mngmt",
      backup_schedule:"Daily @ 11:37 UTC"
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
      maintenance_window:"Sundays, 2 AM - 4 AM",
      recent_changes:[
        {date:"2025-03", description:"Initial documentation system deployed"},
        {date:"2025-03", description:"Traefik reverse proxy configured"},
        {date:"2025-03", description:"Watchtower enabled for automated updates"}
      ]
    }
    
    @dependencies{
      depends_on:["network infrastructure", "DNS server"],
      depended_on_by:["all homelab services"]
    }
    
    @contact{
      email:$CONTACT,
      responsibility:"issues, requests, or contributions"
    }
    
    <@services>
      @service{
        name:"MkDocs Material",
        type:"documentation",
        description:"documentation framework that renders Markdown as documentation sites",
        container:"squidfunk/mkdocs-material",
        ports:["8001-8010:8000"],
        instances:[
          {name:"docs-doc-box-53", port:"8001:8000", path:"/doc-box-53"},
          {name:"docs-arc4d3", port:"8002:8000", path:"/arc4d3"}
        ]
      }
      
      @service{
        name:"Traefik",
        type:"proxy",
        description:"reverse proxy for centralized routing",
        container:"traefik:latest",
        ports:["80:80", "443:443"],
        config_path:"/srv/docs/traefik/config"
      }
      
      @service{
        name:"Watchtower",
        type:"maintenance",
        description:"Docker image updater",
        container:"containrrr/watchtower",
        parameters:"--interval 30 --cleanup"
      }
    </@services>
    
    <@network_config>
      @config{
        type:"static",
        ip:$IP,
        exposed_ports:["80", "443", "8001-8010"],
        domains:[
          {name:"docs.internal.lab.vm", type:"internal"},
          {name:"docs.local.arc4d3.io", type:"public"}
        ]
      }
    </@network_config>
    
    <@file_structure>
      @directory{
        path:"/srv/docs",
        owner:"pals",
        contains:[
          {
            file:"docker-compose.yml",
            purpose:"container orchestration",
            content_type:"yaml"
          },
          {
            path:"repos",
            purpose:"documentation repositories",
            contains:[
              {
                path:"arc4d3",
                purpose:"project documentation",
                structure:[
                  "docs/assets/logo.png",
                  "docs/index.md",
                  "docs/reference/*",
                  "mkdocs.yml",
                  "overrides/main.html"
                ]
              },
              {
                path:"internal.lab.vm.doc-box-53",
                purpose:"self-documentation",
                structure:[
                  "docs/assets/debian.png",
                  "docs/index.md",
                  "docs/reference/*",
                  "docs/services/*",
                  "docs/maintenance/*",
                  "docs/tldr/*",
                  "mkdocs.yml",
                  "README.md"
                ]
              }
            ]
          }
        ]
      }
    </@file_structure>
    
    <@configuration>
      <@docker_compose>
        @template{
          version:"3",
          services:[
            {
              name:"doc-box-53",
              image:"squidfunk/mkdocs-material",
              container_name:"docs-doc-box-53",
              ports:["8001:8000"],
              volumes:["./repos/internal.lab.vm.doc-box-53:/docs"],
              restart:"unless-stopped",
              labels:[
                "traefik.enable=true",
                "traefik.http.routers.doc-box-53.rule=Host(`docs.internal.lab.vm`) && PathPrefix(`/doc-box-53`)",
                "traefik.http.routers.doc-box-53.entrypoints=web,websecure",
                "traefik.http.services.doc-box-53.loadbalancer.server.port=8000"
              ]
            },
            {
              name:"arc4d3",
              image:"squidfunk/mkdocs-material",
              container_name:"docs-arc4d3",
              ports:["8002:8000"],
              volumes:["./repos/arc4d3:/docs"],
              restart:"unless-stopped",
              labels:[
                "traefik.enable=true",
                "traefik.http.routers.arc4d3.rule=Host(`docs.internal.lab.vm`) && PathPrefix(`/arc4d3`)",
                "traefik.http.routers.arc4d3.entrypoints=web,websecure",
                "traefik.http.services.arc4d3.loadbalancer.server.port=8000"
              ]
            },
            {
              name:"traefik",
              image:"traefik:latest",
              container_name:"traefik",
              ports:["80:80", "443:443"],
              volumes:[
                "/var/run/docker.sock:/var/run/docker.sock",
                "./traefik/config:/etc/traefik"
              ],
              restart:"unless-stopped"
            },
            {
              name:"watchtower",
              image:"containrrr/watchtower",
              container_name:"watchtower",
              volumes:["/var/run/docker.sock:/var/run/docker.sock"],
              command:"--interval 30 --cleanup",
              restart:"unless-stopped"
            }
          ]
        }
      </@docker_compose>
      
      <@mkdocs>
        @template{
          site_name:"VM doc-box-53",
          nav:[
            {section:"Home", path:"index.md"},
            {
              section:"Reference",
              items:[
                {title:"Purpose", path:"reference/purpose.md"},
                {title:"Server Specifications", path:"reference/server.md"},
                {title:"Application", path:"reference/application.md"},
                {title:"Guidelines", path:"reference/guidelines.md"},
                {title:"Templates", path:"reference/templates.md"}
              ]
            },
            {
              section:"Services",
              items:[
                {title:"Docker Containers", path:"services/docker.md"}
              ]
            },
            {
              section:"Maintenance",
              items:[
                {title:"Backup Procedures", path:"maintenance/backups.md"},
                {title:"Update Process", path:"maintenance/updates.md"}
              ]
            },
            {
              section:"TLDR",
              items:[
                {title:"Summary", path:"tldr/tldr.md"}
              ]
            }
          ],
          theme:{
            name:"material",
            logo:"assets/debian.png",
            features:[
              "content.code.copy",
              "navigation.instant",
              "navigation.tracking",
              "navigation.expand",
              "navigation.indexes",
              "toc.follow"
            ],
            palette:{
              primary:"indigo",
              accent:"indigo"
            }
          },
          markdown_extensions:[
            "abbr",
            "admonition",
            "attr_list",
            "def_list",
            "footnotes",
            "md_in_html",
            "toc",
            "pymdownx.arithmatex",
            "pymdownx.betterem",
            "pymdownx.caret",
            "pymdownx.details",
            "pymdownx.emoji",
            "pymdownx.highlight",
            "pymdownx.inlinehilite",
            "pymdownx.keys",
            "pymdownx.magiclink",
            "pymdownx.mark",
            "pymdownx.smartsymbols",
            "pymdownx.superfences",
            "pymdownx.tabbed",
            "pymdownx.tasklist",
            "pymdownx.tilde"
          ]
        }
      </@mkdocs>
    </@configuration>
    
    <@maintenance>
      <@backup>
        @policy{
          schedule:"Daily @ 11:37 UTC",
          retention:"7 days",
          location:"PBS storage on pbs-box-73",
          type:"Full VM backup",
          verification:"Weekly automated backup verification"
        }
        
        @content_backup{
          git_repositories:{
            frequency:"With each commit",
            location:"GitLab instance (git.internal.lab.vm)",
            included_in:"GitLab backup procedure"
          },
          manual_export:{
            frequency:"Monthly",
            script:"/usr/local/bin/export-docs.sh",
            command:"tar -czf /backup/doc-exports/docs-export-$(date +%Y%m%d).tar.gz repos/",
            location:"/backup/doc-exports/"
          }
        }
        
        @restoration{
          vm_procedure:[
            "Access the Proxmox web interface",
            "Navigate to the Backup section",
            "Select the appropriate backup of doc-box-53",
            "Click \"Restore\" and select the target node",
            "Verify network settings before restoring"
          ],
          content_procedure:{
            git_restore:[
              "git clone http://git.internal.lab.vm/homelab/internal.lab.vm.doc-box-53.git"
            ],
            archive_restore:[
              "tar -xzf /backup/doc-exports/docs-export-YYYYMMDD.tar.gz -C /tmp/",
              "cp -r /tmp/repos/internal.lab.vm.doc-box-53 /srv/docs/repos/",
              "cd /srv/docs",
              "docker-compose restart doc-box-53"
            ]
          }
        }
        
        @monitoring{
          methods:[
            "Daily email reports from Proxmox backup jobs",
            "Monitoring dashboard alerts for failed backups",
            "Weekly backup verification tests"
          ],
          emergency_contacts:[
            {role:"Primary", contact:$CONTACT},
            {role:"Secondary", contact:"Infrastructure team via internal chat"}
          ]
        }
      </@backup>
      
      <@updates>
        @schedule{
          security:{
            frequency:"Weekly",
            method:"Automated",
            mechanism:"Unattended-upgrades package"
          },
          system:{
            frequency:"Monthly",
            method:"Manual",
            timing:"During maintenance window"
          },
          os_version:{
            frequency:"As needed",
            method:"Manual",
            planning:"Announced 2 weeks in advance"
          }
        }
        
        @procedure{
          manual_steps:[
            "sudo apt update",
            "sudo apt upgrade -y",
            "if [ -f /var/run/reboot-required ]; then echo \"Reboot required\"; sudo shutdown -r 02:00; fi"
          ],
          docker:{
            method:"Automatic via Watchtower",
            frequency:"Every 30 minutes",
            logging:"System journal"
          },
          documentation:{
            method:"Git-based content updates",
            trigger:"Git push",
            action:"Automatic container rebuild"
          }
        }
        
        @rollback{
          system:[
            "sudo apt install ppa-purge",
            "sudo ppa-purge <problematic-ppa>",
            "Alternatively, restore from VM backup"
          ],
          docker:[
            "docker compose down",
            "Edit docker-compose.yml to specify previous version",
            "docker compose up -d"
          ],
          documentation:[
            "git revert <commit-hash>",
            "git push"
          ]
        }
        
        @notifications{
          channels:[
            "Internal notification system for planned maintenance",
            "Changelog entries in the documentation",
            "System status dashboard updates"
          ],
          maintenance_window:{
            day:"Sundays",
            time:"2 AM - 4 AM",
            frequency:"Weekly"
          }
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
          "docs/reference/server.md",
          "docs/reference/purpose.md",
          "docs/reference/network.md",
          "docs/reference/maintenance/backups.md",
          "docs/services/docker.md",
          "docs/assets/logo.png",
          "mkdocs.yml"
        ]
      }
      
      @guidelines{
        style:[
          "Be concise and use clear, direct language",
          "Avoid unnecessary technical jargon",
          "Break up content with headings and lists",
          "Use consistent Markdown formatting",
          "Use emoji icons consistently",
          "Use code blocks for commands and configuration snippets"
        ],
        required_sections:[
          "Purpose and business justification",
          "Server specifications",
          "Network configuration",
          "Services running",
          "Maintenance procedures",
          "Dependency relationships"
        ],
        update_policy:[
          "Update documentation whenever changes are made to the VM",
          "Include a \"Last Updated\" date in each major section",
          "Document changes in the \"Recent Changes\" section of index.md"
        ]
      }
      
      @contribution_process{
        steps:[
          "Clone the repository for the VM documentation",
          "Make changes following these guidelines",
          "Test locally using Docker",
          "Commit and push changes",
          "Watchtower will automatically deploy the updated documentation"
        ]
      }
    </@documentation_structure>
    
    <@access>
      @endpoints{
        direct:"http://10.10.10.53:8000",
        proxy:"https://docs.local.arc4d3.io",
        internal_ports:"8001-8010",
        external_port:"443"
      }
      
      @useful_links{
        pbs_dashboard:"https://192.168.199.73:8007/#pbsDashboard",
        pbs_vm_view:"https://192.168.199.2:8006/#v1:0:=qemu%2F102:4:::::::",
        internal_access:"http://10.10.10.53:8000",
        mkdocs_material_docs:"https://squidfunk.github.io/mkdocs-material/",
        traefik_documentation:"https://doc.traefik.io/traefik/",
        public_access:"https://docs.local.arc4d3.io"
      }
    </@access>
    
    <@development_workflow>
      @process{
        steps:[
          "Content is written in Markdown within the appropriate repository structure",
          "Changes are committed to the Git repository",
          "Watchtower automatically detects changes and rebuilds the documentation container",
          "Documentation is updated and available via the Traefik reverse proxy"
        ]
      }
      
      @new_vm_documentation{
        steps:[
          "Copy the template directory structure to a new repos/internal.lab.vm.{vm-name} folder",
          "Edit mkdocs.yml, index.md, and files inside reference/",
          "Add a new service entry to the main docker-compose.yml on the host VM",
          "Push changes to Git – this triggers the rebuild via watchtower"
        ]
      }
    </@development_workflow>
  </@entity>
</@context>

&meta{author:"ISCP-60 Conversion", date:"2025-03-25", version:"1.0", purpose:"machine_interpretable_documentation", completeness:"full"}