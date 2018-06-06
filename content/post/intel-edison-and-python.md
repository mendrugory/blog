---
title: "Intel Edison and Python"
date: 2015-03-07
url: /post/intel-edison-and-python
index: true
---

<div class="blog-content">
 <div>
 <div class="wsite-multicol">
  <div class="wsite-multicol-table-wrap" style="margin:0 -15px;">
   <table class="wsite-multicol-table">
    <tbody class="wsite-multicol-tbody">
     <tr class="wsite-multicol-tr">
      <td class="wsite-multicol-col" style="width:50%; padding:0 15px;">
       <div>
        <div class="wsite-image wsite-image-border-thin " style="padding-top:10px;padding-bottom:10px;margin-left:0;margin-right:0;text-align:center">
         <a>
          <img alt="Imagen" src="/img/5029974.jpeg" style="width:214;max-width:100%"/>
         </a>
         <div style="display:block;font-size:90%">
         </div>
        </div>
       </div>
      </td>
      <td class="wsite-multicol-col" style="width:50%; padding:0 15px;">
       <div class="paragraph" style="text-align:left;">
        <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
         <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
          <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
           <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
            <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
             <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
              <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
               <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                  I have begun to work with
                 <a href="https://www-ssl.intel.com/content/www/us/en/do-it-yourself/edison.html?_ga=1.127944651.638406789.1424627868" target="_blank" title="">
                   Intel Edison
                 </a>
                  in order to adapt machines with limited or even none connectivity to the movement of
                 <a href="http://en.wikipedia.org/wiki/Internet_of_Things" target="_blank" title="">
                   IoT
                 </a>
                  .
                 <br/>
                  This tiny platform is already prepared to work with Python but, if you want to work with Python, you want to work with
                 <a href="http://en.wikipedia.org/wiki/Pip_%28package_manager%29" title="">
                   pip
                 </a>
                  . Pip is an amazing package manager where we can find almost all python libraries that we are going to use.
                 <br/>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </div>
      </td>
     </tr>
    </tbody>
   </table>
  </div>
 </div>
 </div>
 <h2 class="wsite-content-title" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:700; color:rgb(73, 68, 66); ">
   Getting Started
  <span style="text-decoration:none; font-style:normal; font-weight:700; color:rgb(73, 68, 66); ">
   <span style="text-decoration:none; font-style:normal; font-weight:700; color:rgb(73, 68, 66); ">
    <span style="text-decoration:none; font-style:normal; font-weight:700; color:rgb(73, 68, 66); ">
     <span style="text-decoration:none; font-style:normal; font-weight:700; color:rgb(73, 68, 66); ">
      <br/>
     </span>
    </span>
   </span>
  </span>
 </span>
 </h2>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
    <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
     <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
      <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
       <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
        <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
         <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
           Firstly we connect two micro-usb cables to the mini breakout board and we run a Linux Terminal.
          <br/>
          <br/>
           The easiest way to communicate to the board is through the Serial Port:
          <br/>
         </span>
        </span>
       </span>
      </span>
     </span>
    </span>
   </span>
  </span>
 </span>
 </div>
 <div class="paragraph" style="text-align:justify;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
    <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
     <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
      <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
       <strong>
        <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
         <font color="#2a2a2a">
          <span style="text-decoration: none; font-style: normal; font-weight: 400;">
           <span style="text-decoration: none; font-style: normal; font-weight: 400;">
            <span style="text-decoration: none; font-style: normal; font-weight: 400;">
             <strong>
              <em>
               <font size="4">
                <span style="text-decoration: none; font-style: normal; font-weight: 400;">
                 <span class="rangySelectionBoundary" id="selectionBoundary_1425715346435_22525410726356077" style="line-height: 0; display: none;">
                   ﻿
                 </span>
                </span>
               </font>
              </em>
             </strong>
            </span>
           </span>
          </span>
         </font>
        </span>
       </strong>
      </span>
     </span>
     <span style="">
      <span style="">
       <strong style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <strong style="">
             <em style="">
              <span style="">
               <em style="">
                <span style="">
                 <font color="#2a2a2a">
                   user@machine:~$ sudo screen /dev/ttyUSB0 115200
                 </font>
                </span>
               </em>
              </span>
             </em>
            </strong>
           </span>
          </span>
         </span>
        </span>
       </strong>
      </span>
     </span>
     <span style="">
      <span style="">
       <strong style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <strong style="">
             <em style="">
              <span style="">
               <em style="">
                <span style="">
                </span>
               </em>
              </span>
             </em>
            </strong>
           </span>
          </span>
         </span>
        </span>
       </strong>
      </span>
     </span>
    </span>
   </span>
  </span>
 </span>
 </div>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
    <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
     <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
      <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
        Press Enter and you’ll get a login prompt. Type
       <font color="#2a2a2a">
        <em>
         <strong>
           root
         </strong>
        </em>
       </font>
        and no password is needed. Now you are connected to the Edison through the Serial port.
       <br/>
        To activate the WiFi is very easy, type:
       <br/>
      </span>
     </span>
    </span>
   </span>
  </span>
 </span>
 </div>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
    <font color="#2a2a2a">
     <em>
      <strong>
       <span style="text-decoration: none; font-style: normal; font-weight: 400;">
        <span style="text-decoration: none; font-style: normal; font-weight: 400;">
         <span style="text-decoration: none; font-style: normal; font-weight: 400;">
          <span style="">
           <span style="">
            <strong style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <strong style="">
                  <em style="">
                   <span style="">
                    <em style="">
                     <span style="">
                       root@edison:~#
                     </span>
                    </em>
                   </span>
                  </em>
                 </strong>
                </span>
               </span>
              </span>
             </span>
            </strong>
           </span>
          </span>
         </span>
        </span>
       </span>
       <span style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <strong style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <strong style="">
                  <em style="">
                   <span style="">
                    <em style="">
                     <span style="">
                     </span>
                    </em>
                   </span>
                  </em>
                 </strong>
                </span>
               </span>
              </span>
             </span>
            </strong>
           </span>
          </span>
         </span>
        </span>
       </span>
        configure_edison --wifi
      </strong>
     </em>
    </font>
    <br/>
   </span>
  </span>
 </span>
 </div>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
     Wait for a few seconds, select your network and type your password.
    <br/>
    <br/>
   </span>
  </span>
 </span>
 </div>
 <h2 class="wsite-content-title" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:700; color:rgb(73, 68, 66); ">
   Add Unofficial Repository
 </span>
 </h2>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
    <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
     <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
       Intel Edison is powered by
      <a href="https://www.yoctoproject.org/" title="">
        Yocto
      </a>
       but pip is not in the official repositories, therefore we need to add the unofficial repositores.
      <br/>
       We began getting the latest
     </span>
    </span>
   </span>
  </span>
  <span style="">
   <span style="">
    <span style="">
     <span style="">
     </span>
    </span>
   </span>
  </span>
  <a href="https://software.intel.com/en-us/articles/managing-devkit-libraries-intel-edison-or-intel-galileo-board" target="_blank">
    Intel® IoT Developer Kit libraries
  </a>
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
    <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
     <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
       .
      <br/>
     </span>
    </span>
   </span>
  </span>
 </span>
 </div>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
    <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
     <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
      <strong>
       <font color="#2a2a2a">
        <span style="text-decoration: none; font-style: normal; font-weight: 400;">
        </span>
       </font>
      </strong>
      <em>
       <font color="#2a2a2a">
        <strong>
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <em style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <em style="">
                             <span style="">
                              <em style="">
                               <span style="">
                                 root@edison:~#
                               </span>
                              </em>
                             </span>
                            </em>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </span>
                  </span>
                 </span>
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <em style="">
                             <span style="">
                              <em style="">
                               <span style="">
                               </span>
                              </em>
                             </span>
                            </em>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </span>
                  </span>
                 </span>
                </span>
               </em>
              </span>
             </span>
            </span>
           </span>
            echo "src intel-iotdk http://iotdk.intel.com/repo/1.1/intelgalactic" &gt; /etc/opkg/intel-iotdk.conf
           <br/>
          </span>
         </span>
        </strong>
       </font>
      </em>
      <em style="">
       <strong style="">
        <span style="">
         <span style="">
         </span>
        </span>
       </strong>
      </em>
      <em>
       <strong>
        <font color="#2a2a2a">
         <em style="">
          <strong style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <em style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <em style="">
                               <span style="">
                                <em style="">
                                 <span style="">
                                   root@edison:~#
                                 </span>
                                </em>
                               </span>
                              </em>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </span>
                  </span>
                 </em>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </strong>
         </em>
          opkg update
         <br/>
         <em style="">
          <strong style="">
           <span style="">
            <span style="">
            </span>
           </span>
          </strong>
         </em>
         <em style="">
          <strong style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <em style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <em style="">
                               <span style="">
                                <em style="">
                                 <span style="">
                                   root@edison:~#
                                 </span>
                                </em>
                               </span>
                              </em>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </span>
                  </span>
                 </em>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </strong>
         </em>
          opkg upgrade
        </font>
       </strong>
      </em>
     </span>
    </span>
   </span>
  </span>
 </span>
 </div>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   Open the vi and add the unofficial repositories:
  <br/>
 </span>
 </div>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
    <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
     <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
      <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
       <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
        <em>
         <font color="#2a2a2a">
          <strong>
           <span style="text-decoration: none; font-style: normal; font-weight: 400;">
            <span style="text-decoration: none; font-style: normal; font-weight: 400;">
             <span style="text-decoration: none; font-style: normal; font-weight: 400;">
              <span style="text-decoration: none; font-style: normal; font-weight: 400;">
               <span style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <strong>
                                    root@edison:~#
                                  </strong>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </span>
                   <strong>
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </strong>
                  </span>
                 </span>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <span style="">
                 </span>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </strong>
         </font>
        </em>
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <strong>
             <em>
              <font color="#2a2a2a">
               <span style="">
                <span style="">
                 <span style="">
                  <span style="">
                    vi /etc/opkg/base-feeds.conf
                  </span>
                 </span>
                </span>
               </span>
               <span style="">
                <span style="">
                </span>
               </span>
              </font>
             </em>
            </strong>
            <span style="">
             <span style="">
              <strong>
               <em>
                <span style="">
                 <font color="#2a2a2a">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </span>
                  </span>
                 </font>
                </span>
               </em>
              </strong>
              <em style="">
               <strong style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <strong style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <strong style="">
                           <em style="">
                            <span style="">
                             <em style="">
                              <span style="">
                               <br/>
                              </span>
                             </em>
                            </span>
                           </em>
                          </strong>
                         </span>
                        </span>
                       </span>
                      </span>
                     </strong>
                    </span>
                   </span>
                  </span>
                 </span>
                </span>
                <br/>
               </strong>
              </em>
              <span style="">
               <span style="">
                 paste the following:
               </span>
              </span>
              <em style="">
               <span style="">
                <br/>
               </span>
              </em>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <span style="">
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
        <font color="#2a2a2a">
         <em>
           src/gz all http://repo.opkg.net/edison/repo/all
          <br/>
           src/gz edison http://repo.opkg.net/edison/repo/edison
          <br/>
           src/gz core2-32 http://repo.opkg.net/edison/repo/core2-32
          <br/>
          <br/>
         </em>
        </font>
       </span>
        close the file and update:
       <br/>
       <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
        <em>
         <font color="#2a2a2a">
          <strong>
           <span style="text-decoration: none; font-style: normal; font-weight: 400;">
            <span style="text-decoration: none; font-style: normal; font-weight: 400;">
             <span style="text-decoration: none; font-style: normal; font-weight: 400;">
              <span style="text-decoration: none; font-style: normal; font-weight: 400;">
               <span style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <strong>
                                    root@edison:~#
                                  </strong>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </span>
                   <strong>
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </strong>
                  </span>
                 </span>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <span style="">
                 </span>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </strong>
         </font>
        </em>
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <strong>
             <em>
              <font color="#2a2a2a">
               <span style="">
                <span style="">
                 <span style="">
                  <span style="">
                    opkg update
                  </span>
                 </span>
                </span>
               </span>
              </font>
             </em>
            </strong>
            <span style="">
             <span style="">
              <em style="">
               <strong style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <strong style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <strong style="">
                           <em style="">
                            <span style="">
                             <em style="">
                              <span style="">
                               <br/>
                               <br/>
                              </span>
                             </em>
                            </span>
                           </em>
                          </strong>
                         </span>
                        </span>
                       </span>
                      </span>
                     </strong>
                    </span>
                   </span>
                  </span>
                 </span>
                </span>
               </strong>
              </em>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
      </span>
     </span>
    </span>
   </span>
  </span>
 </span>
 </div>
 <h2 class="wsite-content-title" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:700; color:rgb(73, 68, 66); ">
   Install Pip
  <br/>
 </span>
 </h2>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
    <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
     <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
      <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
       <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
        <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
         <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
          <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
           <em>
            <font color="#2a2a2a">
             <strong>
              <span style="text-decoration: none; font-style: normal; font-weight: 400;">
               <span style="text-decoration: none; font-style: normal; font-weight: 400;">
                <span style="text-decoration: none; font-style: normal; font-weight: 400;">
                 <span style="text-decoration: none; font-style: normal; font-weight: 400;">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                     <strong>
                                       root@edison:~#
                                     </strong>
                                    </span>
                                   </span>
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                      <strong>
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                     </span>
                                    </span>
                                   </span>
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </strong>
                     </span>
                    </span>
                   </span>
                  </span>
                 </span>
                </span>
               </span>
              </span>
              <span style="">
               <span style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                    </span>
                   </span>
                  </span>
                 </span>
                </span>
               </span>
              </span>
             </strong>
            </font>
           </em>
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <strong>
                <em>
                 <font color="#2a2a2a">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                       opkg install python-pip
                     </span>
                    </span>
                   </span>
                  </span>
                 </font>
                </em>
               </strong>
               <span style="">
                <span style="">
                 <strong>
                  <em>
                   <span style="">
                    <font color="#2a2a2a">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                    </span>
                                   </span>
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </font>
                   </span>
                  </em>
                 </strong>
                 <em style="">
                  <strong style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <strong style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <strong style="">
                              <em style="">
                               <span style="">
                                <em style="">
                                 <span style="">
                                  <br/>
                                 </span>
                                </em>
                               </span>
                              </em>
                             </strong>
                            </span>
                           </span>
                          </span>
                         </span>
                        </strong>
                       </span>
                      </span>
                     </span>
                    </span>
                   </span>
                  </strong>
                 </em>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
      </span>
     </span>
    </span>
   </span>
  </span>
 </span>
 </div>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
    <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
     <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
      <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
        After running the above command, everything seems to be fine. You already have your pip and you will be able to install everything that you want, for example
       <a href="http://flask.pocoo.org/" target="_blank" title="">
         Flask
       </a>
        .
       <br/>
      </span>
     </span>
    </span>
   </span>
  </span>
 </span>
 </div>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
    <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
     <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
      <font color="#2a2a2a">
       <span style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <em style="">
              <strong style="">
               <span style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                      <strong style="">
                                        root@edison:~#
                                      </strong>
                                     </span>
                                    </span>
                                   </span>
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                       <strong style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                      <span style="">
                                      </span>
                                     </span>
                                    </span>
                                   </span>
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </strong>
                      </span>
                     </span>
                    </span>
                   </span>
                  </span>
                 </span>
                </span>
               </span>
               <span style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                     </span>
                    </span>
                   </span>
                  </span>
                 </span>
                </span>
               </span>
              </strong>
             </em>
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <strong style="">
                  <em style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                        pip install flask
                       <br/>
                      </span>
                     </span>
                    </span>
                   </span>
                  </em>
                 </strong>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
       <span style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <strong style="">
                  <em style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                        Traceback (most recent call last):
                       <br/>
                      </span>
                     </span>
                    </span>
                   </span>
                  </em>
                 </strong>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
       <span style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <strong style="">
                  <em style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                      </span>
                     </span>
                    </span>
                   </span>
                  </em>
                 </strong>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
       <span style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <strong style="">
                  <em style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <strong style="">
                                  <em style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                      <span style="">
                                      </span>
                                     </span>
                                    </span>
                                   </span>
                                  </em>
                                 </strong>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                        File "/usr/bin/pip", line 5, in &lt;module&gt;
                       <br/>
                      </span>
                     </span>
                    </span>
                   </span>
                  </em>
                 </strong>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
       <span style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <strong style="">
                  <em style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                      </span>
                     </span>
                    </span>
                   </span>
                  </em>
                 </strong>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
       <span style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <strong style="">
                  <em style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <strong style="">
                                  <em style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                      <span style="">
                                      </span>
                                     </span>
                                    </span>
                                   </span>
                                  </em>
                                 </strong>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                        from pkg_resources import load_entry_point
                       <br/>
                      </span>
                     </span>
                    </span>
                   </span>
                  </em>
                 </strong>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
       <span style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <strong style="">
                  <em style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                      </span>
                     </span>
                    </span>
                   </span>
                  </em>
                 </strong>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
      </font>
      <span style="">
       <span style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <strong style="">
                 <em style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <font color="#2a2a2a">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <strong style="">
                                  <em style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                      <span style="">
                                      </span>
                                     </span>
                                    </span>
                                   </span>
                                  </em>
                                 </strong>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                        ImportError: No module named pkg_resources
                      </font>
                      <br/>
                     </span>
                    </span>
                   </span>
                  </span>
                 </em>
                </strong>
                <span style="">
                 <span style="">
                  <strong style="">
                   <em style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                    </span>
                                   </span>
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </em>
                  </strong>
                  <em style="">
                   <strong style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <strong style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <strong style="">
                               <em style="">
                                <span style="">
                                </span>
                               </em>
                              </strong>
                             </span>
                            </span>
                           </span>
                          </span>
                         </strong>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </strong>
                  </em>
                 </span>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
      </span>
     </span>
    </span>
   </span>
  </span>
 </span>
 </div>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
    <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
     <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
      <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
       <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
        <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
         <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
          <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
           <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
            <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
             <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
              <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
               <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                    But we get an error. This error means that we do not have
                   <a href="https://pypi.python.org/pypi/setuptools" title="">
                     setuptools.py
                   </a>
                    , so we try to install. If we try to use
                   <em>
                     wget
                   </em>
                    , the Edison will not be able to download because it can not download from https. Therefore, we could download to our PC and use
                   <em>
                     scp
                   </em>
                    to copy the library to the board.
                   <br/>
                  </span>
                 </span>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
      </span>
     </span>
    </span>
   </span>
  </span>
 </span>
 </div>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
    <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
     <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
      <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
       <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
        <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
         <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
          <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
           <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
            <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
             <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
              <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
               <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <em style="">
                             <strong style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                      <span style="">
                                       <span style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                           <span style="">
                                            <span style="">
                                             <span style="">
                                              <span style="">
                                               <span style="">
                                                <span style="">
                                                 <span style="">
                                                  <span style="">
                                                   <span style="">
                                                    <span style="">
                                                     <strong style="">
                                                      <font color="#2a2a2a">
                                                        root@edison:~#
                                                      </font>
                                                     </strong>
                                                    </span>
                                                   </span>
                                                  </span>
                                                 </span>
                                                </span>
                                               </span>
                                              </span>
                                             </span>
                                            </span>
                                           </span>
                                          </span>
                                         </span>
                                        </span>
                                       </span>
                                      </span>
                                      <font color="#2a2a2a">
                                       <strong style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                           <span style="">
                                            <span style="">
                                             <span style="">
                                              <span style="">
                                               <span style="">
                                                <span style="">
                                                 <span style="">
                                                  <span style="">
                                                   <span style="">
                                                    <span style="">
                                                     <span style="">
                                                      <span style="">
                                                      </span>
                                                     </span>
                                                    </span>
                                                   </span>
                                                  </span>
                                                 </span>
                                                </span>
                                               </span>
                                              </span>
                                             </span>
                                            </span>
                                           </span>
                                          </span>
                                         </span>
                                        </span>
                                       </strong>
                                      </font>
                                     </span>
                                    </span>
                                   </span>
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </span>
                              <font color="#2a2a2a">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                     </span>
                                    </span>
                                   </span>
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </font>
                             </strong>
                            </em>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                    <font color="#2a2a2a">
                     <em>
                      <strong>
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                                scp user@machine:/home/user/Download/setuptools-12.2.tar.gz /home/root
                               <br/>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </strong>
                     </em>
                    </font>
                   </span>
                  </span>
                 </span>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
          <span style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <em style="">
                     <strong style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </strong>
                    </em>
                   </span>
                  </span>
                 </span>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
          <span style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <em style="">
                     <strong style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                   <em style="">
                                    <strong style="">
                                     <span style="">
                                      <span style="">
                                       <span style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                           <span style="">
                                            <span style="">
                                             <span style="">
                                              <em style="">
                                               <strong style="">
                                                <span style="">
                                                 <span style="">
                                                  <span style="">
                                                   <span style="">
                                                    <span style="">
                                                     <span style="">
                                                      <span style="">
                                                       <span style="">
                                                        <span style="">
                                                         <span style="">
                                                          <span style="">
                                                           <span style="">
                                                            <span style="">
                                                             <span style="">
                                                              <span style="">
                                                               <span style="">
                                                                <span style="">
                                                                 <span style="">
                                                                  <span style="">
                                                                   <span style="">
                                                                    <span style="">
                                                                     <span style="">
                                                                      <span style="">
                                                                       <strong style="">
                                                                        <font color="#2a2a2a">
                                                                          root@edison:~#
                                                                        </font>
                                                                       </strong>
                                                                      </span>
                                                                     </span>
                                                                    </span>
                                                                   </span>
                                                                  </span>
                                                                 </span>
                                                                </span>
                                                               </span>
                                                              </span>
                                                             </span>
                                                            </span>
                                                           </span>
                                                          </span>
                                                         </span>
                                                        </span>
                                                        <font color="#2a2a2a">
                                                         <strong style="">
                                                          <span style="">
                                                           <span style="">
                                                            <span style="">
                                                             <span style="">
                                                              <span style="">
                                                               <span style="">
                                                                <span style="">
                                                                 <span style="">
                                                                  <span style="">
                                                                   <span style="">
                                                                    <span style="">
                                                                     <span style="">
                                                                      <span style="">
                                                                       <span style="">
                                                                        <span style="">
                                                                        </span>
                                                                       </span>
                                                                      </span>
                                                                     </span>
                                                                    </span>
                                                                   </span>
                                                                  </span>
                                                                 </span>
                                                                </span>
                                                               </span>
                                                              </span>
                                                             </span>
                                                            </span>
                                                           </span>
                                                          </span>
                                                         </strong>
                                                        </font>
                                                       </span>
                                                      </span>
                                                     </span>
                                                    </span>
                                                   </span>
                                                  </span>
                                                 </span>
                                                </span>
                                                <font color="#2a2a2a">
                                                 <span style="">
                                                  <span style="">
                                                   <span style="">
                                                    <span style="">
                                                     <span style="">
                                                      <span style="">
                                                       <span style="">
                                                       </span>
                                                      </span>
                                                     </span>
                                                    </span>
                                                   </span>
                                                  </span>
                                                 </span>
                                                </font>
                                               </strong>
                                              </em>
                                              <font color="#2a2a2a">
                                               <span style="">
                                                <span style="">
                                                 <span style="">
                                                  <span style="">
                                                   <strong style="">
                                                    <em style="">
                                                     <span style="">
                                                      <span style="">
                                                       <span style="">
                                                        <span style="">
                                                        </span>
                                                       </span>
                                                      </span>
                                                     </span>
                                                    </em>
                                                   </strong>
                                                  </span>
                                                 </span>
                                                </span>
                                               </span>
                                              </font>
                                             </span>
                                            </span>
                                           </span>
                                          </span>
                                         </span>
                                        </span>
                                       </span>
                                      </span>
                                     </span>
                                     <font color="#2a2a2a">
                                      <span style="">
                                       <span style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                           <span style="">
                                            <span style="">
                                             <span style="">
                                              <span style="">
                                               <span style="">
                                                <span style="">
                                                 <span style="">
                                                  <span style="">
                                                   <strong style="">
                                                    <em style="">
                                                     <span style="">
                                                      <span style="">
                                                       <span style="">
                                                        <span style="">
                                                        </span>
                                                       </span>
                                                      </span>
                                                     </span>
                                                    </em>
                                                   </strong>
                                                  </span>
                                                 </span>
                                                </span>
                                               </span>
                                              </span>
                                             </span>
                                            </span>
                                           </span>
                                          </span>
                                         </span>
                                        </span>
                                       </span>
                                      </span>
                                      <span style="">
                                       <span style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                           <span style="">
                                            <span style="">
                                             <span style="">
                                              <span style="">
                                               <span style="">
                                                <span style="">
                                                 <span style="">
                                                  <span style="">
                                                   <strong style="">
                                                    <em style="">
                                                     <span style="">
                                                      <span style="">
                                                       <span style="">
                                                        <span style="">
                                                         <strong style="">
                                                           tar zxf
                                                         </strong>
                                                        </span>
                                                       </span>
                                                      </span>
                                                     </span>
                                                    </em>
                                                   </strong>
                                                  </span>
                                                 </span>
                                                </span>
                                               </span>
                                              </span>
                                             </span>
                                            </span>
                                           </span>
                                          </span>
                                         </span>
                                        </span>
                                       </span>
                                      </span>
                                      <span style="">
                                       <span style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                           <span style="">
                                            <span style="">
                                             <span style="">
                                              <span style="">
                                               <span style="">
                                                <span style="">
                                                 <span style="">
                                                  <span style="">
                                                   <strong style="">
                                                    <em style="">
                                                     <span style="">
                                                      <span style="">
                                                       <span style="">
                                                        <span style="">
                                                        </span>
                                                       </span>
                                                      </span>
                                                     </span>
                                                    </em>
                                                   </strong>
                                                  </span>
                                                 </span>
                                                </span>
                                               </span>
                                              </span>
                                             </span>
                                            </span>
                                           </span>
                                          </span>
                                         </span>
                                        </span>
                                       </span>
                                      </span>
                                     </font>
                                    </strong>
                                   </em>
                                   <font color="#2a2a2a">
                                    <span style="">
                                     <span style="">
                                      <span style="">
                                       <span style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                           <span style="">
                                            <span style="">
                                             <span style="">
                                              <span style="">
                                               <span style="">
                                                <span style="">
                                                 <em style="">
                                                  <strong style="">
                                                   <em style="">
                                                    <span style="">
                                                     <span style="">
                                                      <span style="">
                                                       <span style="">
                                                        <span style="">
                                                         <span style="">
                                                          <span style="">
                                                           <span style="">
                                                            <span style="">
                                                             <span style="">
                                                              <span style="">
                                                               <span style="">
                                                                <span style="">
                                                                 <span style="">
                                                                  <span style="">
                                                                    setuptools-12.2.tar.gz
                                                                  </span>
                                                                 </span>
                                                                </span>
                                                               </span>
                                                              </span>
                                                             </span>
                                                            </span>
                                                           </span>
                                                          </span>
                                                         </span>
                                                        </span>
                                                       </span>
                                                      </span>
                                                     </span>
                                                    </span>
                                                   </em>
                                                  </strong>
                                                 </em>
                                                 <span style="">
                                                  <span style="">
                                                   <em style="">
                                                    <strong style="">
                                                     <em style="">
                                                      <span style="">
                                                       <span style="">
                                                        <span style="">
                                                         <span style="">
                                                          <span style="">
                                                           <span style="">
                                                            <span style="">
                                                             <span style="">
                                                              <span style="">
                                                               <span style="">
                                                                <span style="">
                                                                 <span style="">
                                                                  <span style="">
                                                                   <span style="">
                                                                    <span style="">
                                                                     <span style="">
                                                                      <span style="">
                                                                       <br/>
                                                                      </span>
                                                                     </span>
                                                                    </span>
                                                                   </span>
                                                                  </span>
                                                                 </span>
                                                                </span>
                                                               </span>
                                                              </span>
                                                             </span>
                                                            </span>
                                                           </span>
                                                          </span>
                                                         </span>
                                                        </span>
                                                       </span>
                                                      </span>
                                                     </em>
                                                    </strong>
                                                   </em>
                                                  </span>
                                                 </span>
                                                </span>
                                               </span>
                                              </span>
                                             </span>
                                            </span>
                                           </span>
                                          </span>
                                         </span>
                                        </span>
                                       </span>
                                      </span>
                                     </span>
                                    </span>
                                   </font>
                                  </span>
                                 </span>
                                 <font color="#2a2a2a">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                      <span style="">
                                       <span style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                           <span style="">
                                            <span style="">
                                             <span style="">
                                              <span style="">
                                               <span style="">
                                                <span style="">
                                                 <span style="">
                                                  <span style="">
                                                   <em style="">
                                                    <strong style="">
                                                     <em style="">
                                                      <span style="">
                                                       <span style="">
                                                        <span style="">
                                                         <span style="">
                                                          <span style="">
                                                           <span style="">
                                                            <span style="">
                                                             <span style="">
                                                              <span style="">
                                                               <span style="">
                                                                <span style="">
                                                                 <span style="">
                                                                  <span style="">
                                                                   <span style="">
                                                                    <span style="">
                                                                     <span style="">
                                                                      <span style="">
                                                                       <span style="">
                                                                        <span style="">
                                                                         <em style="">
                                                                          <strong style="">
                                                                           <span style="">
                                                                            <span style="">
                                                                             <span style="">
                                                                              <span style="">
                                                                               <span style="">
                                                                                <span style="">
                                                                                 <span style="">
                                                                                  <span style="">
                                                                                   <span style="">
                                                                                    <em style="">
                                                                                     <strong style="">
                                                                                      <span style="">
                                                                                       <span style="">
                                                                                        <span style="">
                                                                                         <span style="">
                                                                                          <span style="">
                                                                                           <span style="">
                                                                                            <span style="">
                                                                                             <span style="">
                                                                                              <span style="">
                                                                                               <span style="">
                                                                                                <span style="">
                                                                                                 <span style="">
                                                                                                  <span style="">
                                                                                                   <span style="">
                                                                                                    <span style="">
                                                                                                     <span style="">
                                                                                                      <span style="">
                                                                                                       <span style="">
                                                                                                        <span style="">
                                                                                                         <span style="">
                                                                                                          <span style="">
                                                                                                           <span style="">
                                                                                                            <span style="">
                                                                                                             <strong style="">
                                                                                                               root@edison:~#
                                                                                                             </strong>
                                                                                                            </span>
                                                                                                           </span>
                                                                                                          </span>
                                                                                                         </span>
                                                                                                        </span>
                                                                                                       </span>
                                                                                                      </span>
                                                                                                     </span>
                                                                                                    </span>
                                                                                                   </span>
                                                                                                  </span>
                                                                                                 </span>
                                                                                                </span>
                                                                                               </span>
                                                                                              </span>
                                                                                              <strong style="">
                                                                                               <span style="">
                                                                                                <span style="">
                                                                                                 <span style="">
                                                                                                  <span style="">
                                                                                                   <span style="">
                                                                                                    <span style="">
                                                                                                     <span style="">
                                                                                                      <span style="">
                                                                                                       <span style="">
                                                                                                        <span style="">
                                                                                                         <span style="">
                                                                                                          <span style="">
                                                                                                           <span style="">
                                                                                                            <span style="">
                                                                                                             <span style="">
                                                                                                             </span>
                                                                                                            </span>
                                                                                                           </span>
                                                                                                          </span>
                                                                                                         </span>
                                                                                                        </span>
                                                                                                       </span>
                                                                                                      </span>
                                                                                                     </span>
                                                                                                    </span>
                                                                                                   </span>
                                                                                                  </span>
                                                                                                 </span>
                                                                                                </span>
                                                                                               </span>
                                                                                              </strong>
                                                                                             </span>
                                                                                            </span>
                                                                                           </span>
                                                                                          </span>
                                                                                         </span>
                                                                                        </span>
                                                                                       </span>
                                                                                      </span>
                                                                                      <span style="">
                                                                                       <span style="">
                                                                                        <span style="">
                                                                                         <span style="">
                                                                                          <span style="">
                                                                                           <span style="">
                                                                                            <span style="">
                                                                                            </span>
                                                                                           </span>
                                                                                          </span>
                                                                                         </span>
                                                                                        </span>
                                                                                       </span>
                                                                                      </span>
                                                                                     </strong>
                                                                                    </em>
                                                                                    <span style="">
                                                                                     <span style="">
                                                                                      <span style="">
                                                                                       <span style="">
                                                                                        <strong style="">
                                                                                         <em style="">
                                                                                          <span style="">
                                                                                           <span style="">
                                                                                            <span style="">
                                                                                             <span style="">
                                                                                             </span>
                                                                                            </span>
                                                                                           </span>
                                                                                          </span>
                                                                                         </em>
                                                                                        </strong>
                                                                                       </span>
                                                                                      </span>
                                                                                     </span>
                                                                                    </span>
                                                                                   </span>
                                                                                  </span>
                                                                                 </span>
                                                                                </span>
                                                                               </span>
                                                                              </span>
                                                                             </span>
                                                                            </span>
                                                                           </span>
                                                                           <span style="">
                                                                            <span style="">
                                                                             <span style="">
                                                                              <span style="">
                                                                               <span style="">
                                                                                <span style="">
                                                                                 <span style="">
                                                                                  <span style="">
                                                                                   <span style="">
                                                                                    <span style="">
                                                                                     <span style="">
                                                                                      <span style="">
                                                                                       <span style="">
                                                                                        <strong style="">
                                                                                         <em style="">
                                                                                          <span style="">
                                                                                           <span style="">
                                                                                            <span style="">
                                                                                             <span style="">
                                                                                             </span>
                                                                                            </span>
                                                                                           </span>
                                                                                          </span>
                                                                                         </em>
                                                                                        </strong>
                                                                                       </span>
                                                                                      </span>
                                                                                     </span>
                                                                                    </span>
                                                                                   </span>
                                                                                  </span>
                                                                                 </span>
                                                                                </span>
                                                                               </span>
                                                                              </span>
                                                                             </span>
                                                                            </span>
                                                                           </span>
                                                                           <span style="">
                                                                            <span style="">
                                                                             <span style="">
                                                                              <span style="">
                                                                               <span style="">
                                                                                <span style="">
                                                                                 <span style="">
                                                                                  <span style="">
                                                                                   <span style="">
                                                                                    <span style="">
                                                                                     <span style="">
                                                                                      <span style="">
                                                                                       <span style="">
                                                                                        <strong style="">
                                                                                         <em style="">
                                                                                          <span style="">
                                                                                           <span style="">
                                                                                            <span style="">
                                                                                             <span style="">
                                                                                             </span>
                                                                                            </span>
                                                                                           </span>
                                                                                          </span>
                                                                                         </em>
                                                                                        </strong>
                                                                                       </span>
                                                                                      </span>
                                                                                     </span>
                                                                                    </span>
                                                                                   </span>
                                                                                  </span>
                                                                                 </span>
                                                                                </span>
                                                                               </span>
                                                                              </span>
                                                                             </span>
                                                                            </span>
                                                                           </span>
                                                                          </strong>
                                                                         </em>
                                                                        </span>
                                                                       </span>
                                                                        python setuptools-12.2/ez_setup.py
                                                                      </span>
                                                                     </span>
                                                                    </span>
                                                                   </span>
                                                                  </span>
                                                                 </span>
                                                                </span>
                                                               </span>
                                                              </span>
                                                             </span>
                                                            </span>
                                                           </span>
                                                          </span>
                                                         </span>
                                                        </span>
                                                       </span>
                                                      </span>
                                                     </em>
                                                    </strong>
                                                   </em>
                                                  </span>
                                                 </span>
                                                </span>
                                               </span>
                                              </span>
                                             </span>
                                            </span>
                                           </span>
                                          </span>
                                         </span>
                                        </span>
                                       </span>
                                      </span>
                                     </span>
                                    </span>
                                   </span>
                                  </span>
                                 </font>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </strong>
                    </em>
                   </span>
                  </span>
                 </span>
                </span>
               </span>
              </span>
             </span>
            </span>
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <em style="">
                                 <strong style="">
                                  <em style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                      <span style="">
                                       <span style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                           <span style="">
                                            <span style="">
                                             <span style="">
                                              <span style="">
                                               <span style="">
                                                <span style="">
                                                 <span style="">
                                                  <span style="">
                                                   <span style="">
                                                    <br/>
                                                   </span>
                                                  </span>
                                                 </span>
                                                </span>
                                               </span>
                                              </span>
                                             </span>
                                            </span>
                                           </span>
                                          </span>
                                         </span>
                                        </span>
                                       </span>
                                      </span>
                                     </span>
                                    </span>
                                   </span>
                                  </em>
                                 </strong>
                                </em>
                                <em style="">
                                 <strong style="">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                      <span style="">
                                       <strong style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                           <span style="">
                                            <strong style="">
                                             <em style="">
                                              <span style="">
                                               <em style="">
                                                <span style="">
                                                </span>
                                               </em>
                                              </span>
                                             </em>
                                            </strong>
                                           </span>
                                          </span>
                                         </span>
                                        </span>
                                       </strong>
                                      </span>
                                     </span>
                                    </span>
                                   </span>
                                  </span>
                                 </strong>
                                </em>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </span>
                  </span>
                 </span>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
      </span>
     </span>
    </span>
   </span>
  </span>
 </span>
 </div>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
    <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
      Or use the insecure way (but faster and easier way):
     <br/>
    </span>
   </span>
  </span>
 </span>
 </div>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="">
    <span style="">
     <span style="">
      <span style="">
       <span style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <em style="">
              <strong style="">
               <span style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <em style="">
                         <strong style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                      <span style="">
                                       <span style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                           <span style="">
                                            <span style="">
                                             <span style="">
                                              <span style="">
                                               <span style="">
                                                <span style="">
                                                 <strong style="">
                                                  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                    <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                     <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                      <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                       <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                        <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                         <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                          <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                           <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                            <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                             <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                              <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                               <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                                <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                                 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
                                                                  <span style="">
                                                                   <span style="">
                                                                    <span style="">
                                                                     <span style="">
                                                                      <span style="">
                                                                       <span style="">
                                                                        <span style="">
                                                                         <span style="">
                                                                          <em style="">
                                                                           <strong style="">
                                                                            <span style="">
                                                                             <span style="">
                                                                              <span style="">
                                                                               <span style="">
                                                                                <span style="">
                                                                                 <span style="">
                                                                                  <span style="">
                                                                                   <span style="">
                                                                                    <span style="">
                                                                                     <span style="">
                                                                                      <span style="">
                                                                                       <span style="">
                                                                                        <span style="">
                                                                                         <span style="">
                                                                                          <span style="">
                                                                                           <span style="">
                                                                                            <span style="">
                                                                                             <span style="">
                                                                                              <span style="">
                                                                                               <span style="">
                                                                                                <span style="">
                                                                                                 <span style="">
                                                                                                  <span style="">
                                                                                                   <strong style="">
                                                                                                    <font color="#2a2a2a">
                                                                                                      root@edison:~#
                                                                                                    </font>
                                                                                                   </strong>
                                                                                                  </span>
                                                                                                 </span>
                                                                                                </span>
                                                                                               </span>
                                                                                              </span>
                                                                                             </span>
                                                                                            </span>
                                                                                           </span>
                                                                                          </span>
                                                                                         </span>
                                                                                        </span>
                                                                                       </span>
                                                                                      </span>
                                                                                     </span>
                                                                                    </span>
                                                                                   </span>
                                                                                  </span>
                                                                                 </span>
                                                                                </span>
                                                                               </span>
                                                                              </span>
                                                                             </span>
                                                                            </span>
                                                                           </strong>
                                                                          </em>
                                                                         </span>
                                                                        </span>
                                                                       </span>
                                                                      </span>
                                                                     </span>
                                                                    </span>
                                                                   </span>
                                                                  </span>
                                                                 </span>
                                                                </span>
                                                               </span>
                                                              </span>
                                                             </span>
                                                            </span>
                                                           </span>
                                                          </span>
                                                         </span>
                                                        </span>
                                                       </span>
                                                      </span>
                                                     </span>
                                                    </span>
                                                   </span>
                                                  </span>
                                                  <font color="#2a2a2a">
                                                    wget --no-check-certificate https://bootstrap.pypa.io/ez_setup.py
                                                   <br/>
                                                  </font>
                                                 </strong>
                                                </span>
                                               </span>
                                              </span>
                                             </span>
                                            </span>
                                           </span>
                                          </span>
                                         </span>
                                        </span>
                                       </span>
                                      </span>
                                     </span>
                                    </span>
                                   </span>
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </strong>
                        </em>
                       </span>
                      </span>
                     </span>
                    </span>
                   </span>
                  </span>
                 </span>
                </span>
               </span>
              </strong>
             </em>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
      </span>
     </span>
    </span>
   </span>
  </span>
  <font color="#2a2a2a">
   <span style="">
    <span style="">
     <span style="">
      <span style="">
       <span style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <span style="">
              <em style="">
               <strong style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <em style="">
                          <strong style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                      <span style="">
                                       <span style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                           <span style="">
                                            <span style="">
                                             <span style="">
                                              <span style="">
                                               <span style="">
                                                <span style="">
                                                 <span style="">
                                                 </span>
                                                </span>
                                               </span>
                                              </span>
                                             </span>
                                            </span>
                                           </span>
                                          </span>
                                         </span>
                                        </span>
                                       </span>
                                      </span>
                                     </span>
                                    </span>
                                   </span>
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </strong>
                         </em>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </span>
                  </span>
                 </span>
                </span>
               </strong>
              </em>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
      </span>
     </span>
    </span>
   </span>
   <span style="">
    <span style="">
     <span style="">
      <span style="">
       <span style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <em style="">
                            <strong style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                      <span style="">
                                       <span style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                           <span style="">
                                            <span style="">
                                             <span style="">
                                              <span style="">
                                               <span style="">
                                                <span style="">
                                                 <span style="">
                                                  <span style="">
                                                   <span style="">
                                                    <strong style="">
                                                      root@edison:~#
                                                    </strong>
                                                   </span>
                                                  </span>
                                                 </span>
                                                </span>
                                               </span>
                                              </span>
                                             </span>
                                            </span>
                                           </span>
                                          </span>
                                         </span>
                                        </span>
                                       </span>
                                      </span>
                                     </span>
                                    </span>
                                   </span>
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </strong>
                           </em>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </span>
                  </span>
                 </span>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
      </span>
     </span>
    </span>
   </span>
   <span style="">
    <span style="">
     <span style="">
      <span style="">
       <span style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <em style="">
                            <strong style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                      <span style="">
                                       <span style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                           <span style="">
                                            <span style="">
                                             <span style="">
                                              <span style="">
                                               <span style="">
                                                <span style="">
                                                 <span style="">
                                                  <span style="">
                                                   <span style="">
                                                    <strong style="">
                                                      python ez_setup.py --insecure
                                                    </strong>
                                                   </span>
                                                  </span>
                                                 </span>
                                                </span>
                                               </span>
                                              </span>
                                             </span>
                                            </span>
                                           </span>
                                          </span>
                                         </span>
                                        </span>
                                       </span>
                                      </span>
                                     </span>
                                    </span>
                                   </span>
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </strong>
                           </em>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </span>
                  </span>
                 </span>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
      </span>
     </span>
    </span>
   </span>
  </font>
 </span>
 </div>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   After having installed setuptools.py we can install whatever we need:
  <br/>
 </span>
 </div>
 <div class="paragraph" style="text-align:left;">
 <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
  <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
   <span style="text-decoration:none; font-style:normal; font-weight:400; color:rgb(121, 117, 112); ">
    <font color="#2a2a2a">
     <span style="text-decoration: none; font-style: normal; font-weight: 400;">
      <strong>
       <span style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <em style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                      <span style="">
                                       <span style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                            root@edison:~#
                                          </span>
                                         </span>
                                        </span>
                                       </span>
                                      </span>
                                     </span>
                                    </span>
                                   </span>
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                           <span style="">
                            <span style="">
                             <span style="">
                              <span style="">
                               <span style="">
                                <span style="">
                                 <span style="">
                                  <span style="">
                                   <span style="">
                                    <span style="">
                                     <span style="">
                                      <span style="">
                                       <span style="">
                                        <span style="">
                                         <span style="">
                                          <span style="">
                                          </span>
                                         </span>
                                        </span>
                                       </span>
                                      </span>
                                     </span>
                                    </span>
                                   </span>
                                  </span>
                                 </span>
                                </span>
                               </span>
                              </span>
                             </span>
                            </span>
                           </span>
                          </span>
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </span>
                   <span style="">
                    <span style="">
                     <span style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                         </span>
                        </span>
                       </span>
                      </span>
                     </span>
                    </span>
                   </span>
                  </span>
                 </em>
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <span style="">
                      <em style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <span style="">
                            pip install flask
                           <br/>
                            ...
                           <br/>
                          </span>
                         </span>
                        </span>
                       </span>
                      </em>
                     </span>
                    </span>
                   </span>
                  </span>
                 </span>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
      </strong>
     </span>
    </font>
    <span style="">
     <strong style="">
      <span style="">
       <span style="">
        <span style="">
         <span style="">
          <span style="">
           <span style="">
            <span style="">
             <span style="">
              <span style="">
               <span style="">
                <span style="">
                 <span style="">
                  <span style="">
                   <span style="">
                    <span style="">
                     <em style="">
                      <span style="">
                       <span style="">
                        <span style="">
                         <span style="">
                          <font color="#2a2a2a">
                            Successfully installed flask
                           <br/>
                            Cleaning up...
                          </font>
                          <br/>
                          <br/>
                         </span>
                        </span>
                       </span>
                      </span>
                     </em>
                    </span>
                   </span>
                  </span>
                 </span>
                </span>
               </span>
              </span>
             </span>
            </span>
           </span>
          </span>
         </span>
        </span>
       </span>
      </span>
     </strong>
    </span>
   </span>
  </span>
 </span>
 </div>
 <div class="wsite-adsense">
 <script src="//www.weebly.com/weebly/apps/serveAds.php?type=adsense&amp;elementid=390664869581035723&amp;ineditor=0&amp;subdomain=mendrugory.weebly.com&amp;pubid=pub-2477529535053270&amp;adformat=468x60&amp;adtype=text_image&amp;bordercolor=FFFFFF&amp;bgcolor=FFFFFF&amp;linkcolor=0F53FF&amp;textcolor=000000&amp;urlcolor=008000" type="text/javascript">
 </script>
 </div>
</div>
