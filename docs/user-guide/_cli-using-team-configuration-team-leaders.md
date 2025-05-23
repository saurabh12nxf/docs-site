# Configuration for team leaders

As a team leader or Dev-Ops advocate, you can share the profiles stored in a team configuration file with your team members so that they can easily access mainframe services.

## Reasons to share team configuration files

Consider sharing a team configuration *globally* in the following scenarios:

- You want to share profiles with application developers so that they can work with a defined set of mainframe services. The recipient of the file places it in their local `~/.zowe` folder manually before issuing CLI commands.
- You want to add the profiles to your project directory in a software change management (SCM) tool, such as GitHub. When you store the profiles in an SCM, application developers can pull the project to their local computer and use the defined configuration. Zowe CLI commands that you issue from within the project directory use the configuration scheme for the project automatically.
- You want to enable test automation in a CI/CD pipeline, which lets your pipelines make use of the project configuration.

For information about how to share team configuration files, see [Sharing team configuration files](../user-guide/cli-using-sharing-team-config-files.md).

## Profile use cases

The following topics describe various profile scenarios that team leaders can share with their teams, and application developers that function as DevOps advocates can create.

### Accessing LPARs that contain services that share the same credentials

In the following configuration, nested profiles use the credentials from the same base profile to access services directly on multiple LPARs: **[is this explanation correct?]**

```
{
    "$schema": "./zowe.schema.json",
    "profiles": {
        "lpar1": {
            "properties": {
                "host": "example1.com"
            },
            "profiles": {
                "zosmf": {
                    "type": "zosmf",
                    "properties": {
                        "port": 443
                    }
                },
                "tso": {
                    "type": "tso",
                    "properties": {
                        "account": "ACCT#",
                        "codePage": "1047",
                        "logonProcedure": "IZUFPROC"
                    }
                },
                "ssh": {
                    "type": "ssh",
                    "properties": {
                        "port": 22
                    }
                }
            }
        },
        "lpar2": {
            "properties": {
                "host": "example2.com"
            },
            "profiles": {
                "zosmf": {
                    "type": "zosmf",
                    "properties": {
                        "port": 1443
                    }
                }
            }
        },
        "base": {
            "type": "base",
            "properties": {
                "rejectUnauthorized": true
            },
            "secure": [
                "user",
                "password"
            ]
        }
    },
    "defaults": {
        "zosmf": "lpar2.zosmf",
        "tso": "lpar1.tso",
        "ssh": "lpar1.ssh",
        "base": "base"
    },
    "autoStore": true
}
```
### Accessing LPARs that contain services that do not share the same credentials

In the following configuration, nested profiles use the credentials from different service profiles to access services directly on multiple LPARs.

```
{
    "$schema": "./zowe.schema.json",
    "profiles": {
        "lpar1": {
            "properties": {
                "host": "example1.com"
            },
            "profiles": {
                "zosmf": {
                    "type": "zosmf",
                    "properties": {
                        "port": 443
                    }
                },
                "tso": {
                    "type": "tso",
                    "properties": {
                        "account": "ACCT#",
                        "codePage": "1047",
                        "logonProcedure": "IZUFPROC"
                    }
                },
                "ssh": {
                    "type": "ssh",
                    "properties": {
                        "port": 22
                    }
                }
            },
            "secure": [
                "user",
                "password"
            ]
        },
        "lpar2": {
            "properties": {
                "host": "example2.com"
            },
            "profiles": {
                "zosmf": {
                    "type": "zosmf",
                    "properties": {
                        "port": 1443
                    }
                }
            },
            "secure": [
                "user",
                "password"
            ]
        },
        "base": {
            "type": "base",
            "properties": {
                "rejectUnauthorized": true
            }
        }
    },
    "defaults": {
        "zosmf": "lpar2.zosmf",
        "tso": "lpar1.tso",
        "ssh": "lpar1.ssh",
        "base": "base"
    },
    "autoStore": true
}
```

### Accessing LPARs that access services through one API Mediation Layer

In the following configuration, services are accessed through API ML (where multi-factor authentication (MFA) or single sign-on (SSO) is achievable) using token-based authorization stored in a base profile.

```
{
    "$schema": "./zowe.schema.json",
    "profiles": {
        "zosmf": {
            "type": "zosmf",
            "properties": {
                "basePath": "api/v1"
            }
        },
        "cics": {
            "type": "cics",
            "properties": {
                "basePath": "api/v1/cics"
            }
        },
        "db2": {
            "type": "db2",
            "properties": {
                "basePath": "api/v1/db2"
            }
        },
        "base": {
            "type": "base",
            "properties": {
                "host": "example.com",
                "port": 7554,
                "rejectUnauthorized": true,
                "tokenType": "apimlAuthenticationToken"
            },
            "secure": [
                "tokenValue"
            ]
        }
    },
    "defaults": {
        "zosmf": "zosmf",
        "cics": "cics",
        "db2": "db2",
        "base": "base"
    },
    "autoStore": true
}
```

### Accessing LPARs that access services through one API Mediation Layer using certificate authentication

In the following configuration, services are accessed through API ML using certificate authentication stored in a base profile.

```
{
    "$schema": "./zowe.schema.json",
    "profiles": {
        "zosmf": {
            "type": "zosmf",
            "properties": {
                "basePath": "api/v1"
            }
        },
        "cics": {
            "type": "cics",
            "properties": {
                "basePath": "api/v1/cics"
            }
        },
        "db2": {
            "type": "db2",
            "properties": {
                "basePath": "api/v1/db2"
            }
        },
        "base": {
            "type": "base",
            "properties": {
                "certFile": "./zowe-cert.pem",
                "certKeyFile": "./zowe-cert-key.pem",
                "host": "example.com",
                "port": 7554,
                "rejectUnauthorized": true
            }
        }
    },
    "defaults": {
        "zosmf": "zosmf",
        "cics": "cics",
        "db2": "db2",
        "base": "base"
    },
    "autoStore": true
}
```