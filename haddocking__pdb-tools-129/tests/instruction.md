# Issue

**Title:** add MANIFEST file to better control which files are packaged to PyPI

## Description

Before, we were packaging tests and missing documentation files when packing to PyPI. Here, I add the `MANIFEST` file to better control which files and folders are packaged. Here's the list. I removed `tests` and added the relevant `docs`. @JoaoRodrigues do you agree? We should merge this with `PATCH` to update the package in PyPI.

```
   pdb-tools-2.4.3/                                                                
   pdb-tools-2.4.3/CODE_OF_CONDUCT.md                                              
   pdb-tools-2.4.3/CONTRIBUTING.md                                                 
   pdb-tools-2.4.3/LICENSE                                                         
   pdb-tools-2.4.3/MANIFEST.in                                                     
   pdb-tools-2.4.3/PKG-INFO                                                        
   pdb-tools-2.4.3/README.md                                                       
   pdb-tools-2.4.3/citation.tex                                                    
   pdb-tools-2.4.3/docs/                                                           
   pdb-tools-2.4.3/docs/cookbook.md                                                
   pdb-tools-2.4.3/docs/index.md                                                   
   pdb-tools-2.4.3/docs/logo/                                                      
   pdb-tools-2.4.3/docs/logo/pdb-tools.png                                         
   pdb-tools-2.4.3/pdb_tools.egg-info/                                             
   pdb-tools-2.4.3/pdb_tools.egg-info/PKG-INFO                                     
   pdb-tools-2.4.3/pdb_tools.egg-info/SOURCES.txt                                  
   pdb-tools-2.4.3/pdb_tools.egg-info/dependency_links.txt                         
   pdb-tools-2.4.3/pdb_tools.egg-info/entry_points.txt                             
   pdb-tools-2.4.3/pdb_tools.egg-info/top_level.txt                                
   pdb-tools-2.4.3/pdbtools/                                                       
   pdb-tools-2.4.3/pdbtools/__init__.py                                            
   pdb-tools-2.4.3/pdbtools/pdb_b.py                                               
   pdb-tools-2.4.3/pdbtools/pdb_chain.py                                           
   pdb-tools-2.4.3/pdbtools/pdb_chainbows.py                                       
   pdb-tools-2.4.3/pdbtools/pdb_chainxseg.py                                       
   pdb-tools-2.4.3/pdbtools/pdb_chkensemble.py                                     
   pdb-tools-2.4.3/pdbtools/pdb_delchain.py                                        
   pdb-tools-2.4.3/pdbtools/pdb_delelem.py                                         
   pdb-tools-2.4.3/pdbtools/pdb_delhetatm.py                                       
   pdb-tools-2.4.3/pdbtools/pdb_delinsertion.py                                    
   pdb-tools-2.4.3/pdbtools/pdb_delres.py                                          
   pdb-tools-2.4.3/pdbtools/pdb_delresname.py                                      
   pdb-tools-2.4.3/pdbtools/pdb_element.py                                         
   pdb-tools-2.4.3/pdbtools/pdb_fetch.py                                           
   pdb-tools-2.4.3/pdbtools/pdb_fixinsert.py                                       
   pdb-tools-2.4.3/pdbtools/pdb_fromcif.py                                         
   pdb-tools-2.4.3/pdbtools/pdb_gap.py                                             
   pdb-tools-2.4.3/pdbtools/pdb_head.py                                            
   pdb-tools-2.4.3/pdbtools/pdb_intersect.py                                       
   pdb-tools-2.4.3/pdbtools/pdb_keepcoord.py                                       
   pdb-tools-2.4.3/pdbtools/pdb_merge.py                                           
   pdb-tools-2.4.3/pdbtools/pdb_mkensemble.py                                      
   pdb-tools-2.4.3/pdbtools/pdb_occ.py                                             
   pdb-tools-2.4.3/pdbtools/pdb_reatom.py                                          
   pdb-tools-2.4.3/pdbtools/pdb_reres.py                                           
   pdb-tools-2.4.3/pdbtools/pdb_rplchain.py                                        
   pdb-tools-2.4.3/pdbtools/pdb_rplresname.py                                      
   pdb-tools-2.4.3/pdbtools/pdb_seg.py                                             
   pdb-tools-2.4.3/pdbtools/pdb_segxchain.py                                       
   pdb-tools-2.4.3/pdbtools/pdb_selaltloc.py                                       
   pdb-tools-2.4.3/pdbtools/pdb_selatom.py                                         
   pdb-tools-2.4.3/pdbtools/pdb_selchain.py                                        
   pdb-tools-2.4.3/pdbtools/pdb_selelem.py                                         
   pdb-tools-2.4.3/pdbtools/pdb_selhetatm.py                                       
   pdb-tools-2.4.3/pdbtools/pdb_selmodel.py                                        
   pdb-tools-2.4.3/pdbtools/pdb_selres.py                                          
   pdb-tools-2.4.3/pdbtools/pdb_selresname.py                                      
   pdb-tools-2.4.3/pdbtools/pdb_selseg.py                                          
   pdb-tools-2.4.3/pdbtools/pdb_shiftres.py                                        
   pdb-tools-2.4.3/pdbtools/pdb_sort.py                                            
   pdb-tools-2.4.3/pdbtools/pdb_splitchain.py                                      
   pdb-tools-2.4.3/pdbtools/pdb_splitmodel.py                                      
   pdb-tools-2.4.3/pdbtools/pdb_splitseg.py                                        
   pdb-tools-2.4.3/pdbtools/pdb_tidy.py                                            
   pdb-tools-2.4.3/pdbtools/pdb_tocif.py                                           
   pdb-tools-2.4.3/pdbtools/pdb_tofasta.py                                         
   pdb-tools-2.4.3/pdbtools/pdb_uniqname.py                                        
   pdb-tools-2.4.3/pdbtools/pdb_validate.py                                        
   pdb-tools-2.4.3/pdbtools/pdb_wc.py                                              
   pdb-tools-2.4.3/setup.cfg                                                       
   pdb-tools-2.4.3/setup.py
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3ffb016b501f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.