
 properties([
                parameters([
                        string(name:'RAKE_ASSETS, choices: [1,0], description: 'If 1 (default) will recompile assets. If in doubt, choose 1.'),
                        string(name: 'REV', defaultValue: "master")     
                        string(name: 'DIST', defaultValue: "el6")
                        string(name: 'JUICER_CART', defaultValue: '', description: "name of Juicer cart. Leave blank to skip cart creation"),
                        string(name: 'JUICER_REPO', defaultValue: 'rhsm-web', description: "name of destination ZPulp repo for RPM")
                ])
        ])
