---
---
[Deutsch](../05a_inventory)

# The Inventory (inventory.json)

The heart of every OCFL object is the `inventory.json`. It contains all the metadata necessary to fully reconstruct the state of the object and its versions.

Here is the `inventory.json` from our workshop example for version `v1`:

```json
{
   "id": "urn:nbn:de:gbv:42-test1",
   "type": "https://ocfl.io/1.1/spec/#inventory",
   "digestAlgorithm": "sha512",
   "head": "v1",
   "contentDirectory": "content",
   "manifest": {
      "0f45fc1448863daf9da4deeed7be5c30fa04e6e82a6d136662c642fcee49f22883795fe747d15e106f61a15e507c8aedb47b32022674763cba49e565f5274140": [
         "v1/content/metadata/thumbnails/v1/00003.png"
      ],
      "1082b5603213566c3d1d2a27146d10bf2f874253ec688a3e8736f40e15a356d3ffdd50036417a246563bc195f4064f2431ab317a4a9bd64edd32d589b87bfcd4": [
         "v1/content/data/image/IMG_6914.jpg"
      ],
      "18c77a1d494a0203a80798132ab8a961894bac15300dec868bc31cecd64c47c9c36e875061b6145c0ae66ce3ab0eb973217217080314475ff5eafd5da62f73dd": [
         "v1/content/data/image/IMG_6914.png"
      ],
      "1bbb24b851ab64ff649ddd038b3597abe3156776b2c53ac92dc72c98e2699d05f2027e32cbd9f5c6b6484a46c4012fbf1a383d0dd8840686878477f72cd2789c": [
         "v1/content/metadata/indexer_v1.jsonl.gz"
      ],
      "228fbd448ccadaa1c7a7b8c6d7bab5bcb9bea862f21317b16f1968abd1cfda97d9833fc1b91f4d77ee0dab52df02576d469a748219f62dd4c49af99aa5f995e2": [
         "v1/content/data/video/Together=u0020Elsewhere=u002001.pdf"
      ],
      "2a078ec51388d5e1c63c490b994b173c2853716afa9a478bc7b234cfa0c839953873b0d19ffa8a264e3bf4230c580ae2db02215189957fd65a0e1ffb508b7f5b": [
         "v1/content/metadata/thumbnails/v1/00002.png"
      ],
      "394e8422bd570224994ff6dabf2a0b7f5e8b9f7817f08ccf42ca0856a3a072e5bc852668f17f69f2c15a75251f03bdcf03a70af280d8699ea6eadb98703c7337": [
         "v1/content/metadata/filesystem_v1.jsonl.gz"
      ],
      "4d3f34495b648372005f544f9cca3d3702fa29b1d89f6a2864d273c37c96cd3ed1773c8b582d6815cedb1a9c89f0a24623ae4ee0608dc1acc4f3f5837e8fbb22": [
         "v1/content/metadata/thumbnails/v1/00010.png"
      ],
      "55d70662e2da382a0a1113b7fa77bb910b50b53f61f1af994af1b09a3bd3069e1a1bc6c975f27e93dbffaf8fc70a67f1bc3e599f2039c468b5689bb32d85a4a0": [
         "v1/content/metadata/thumbnails/v1/00004.png"
      ],
      "56f08a903abf5b2a9feb6bebab95b4dbaad75d04727638cdaad8e172c1245d84efb9a0c99abcd937d2527b3a26452ba57fda9172fc24c012c874522b893ab2b7": [
         "v1/content/data/image/IMG_6914.gif"
      ],
      "5bf773add0abe51fc39f1054ab2e34fceae987e431a8b97c3780d0c0948132cde33c2d7d98b8b94dd49daaf15f564ad2a4663a7181b5f8231e37576fff4a1e67": [
         "v1/content/data/audio/Education=u0020of=u0020the=u0020Noobz=u00201.doc"
      ],
      "6d232473dca41be83138d720c2b5a600c6ec05f5ec46dab04aee93cc2d8834dcc3e35811c484320ca08d5d1554d87a6ec8afd07fb809708dc0c54c3ad473be6b": [
         "v1/content/data/video/together_01_excerpt.mkv"
      ],
      "6f6a6418e72610c0021412627ac89a5d1acb3c90a6fdef7d8c408c96bdbaa2f528b307f99fb72dd83458d796a10601ce012d7749d4b4493c163cd09ec5ce9b89": [
         "v1/content/README.md"
      ],
      "72b182ad228d3092cd3a8fadbb75d739d55fd938f0557d149720bb0b32ae48604b9b8e2f0b0112d5ce67aa6cff512a944eafc90ea5e41570902ec7f2b757584d": [
         "v1/content/metadata/thumbnails/v1/00001.png"
      ],
      "763712ca6ecf1959287c84a29c43f76e89ab83ab3bd901867450ab77e737ee72cdaa3d0a8f4a8224e61736efa6fa5a6b754ef40229f1500edc4b11215d3b3ee9": [
         "v1/content/data/vector/ki.pdf"
      ],
      "7841cd462db95b5354012b866f599bf75540cd7edc904b108cf0e933d79c9b4e6481c511fd2a1506d8223732897cc151735b2b16755db81c6f124ff4fc1ccd28": [
         "v1/content/data/text/export_mets.xml"
      ],
      "791e4839906d0303cbfe97e3f092b5b375210fb9ca25f88e75a937a28e93b16a17ae58c87a79f2ed5d92b6405f29ab5b119d808c598111642d3e1e31c700b182": [
         "v1/content/data/image/IMG_6914.webp"
      ],
      "79f7273c26222b0b843d19495e9c6c31492bf0d7df4e5d161b8d77693504e763698b9f093f1c889f8c85053fecc8dea6bfdfffcdda894c6c3476448543477735": [
         "v1/content/data/audio/Dragan_Espenschied_-_10_-_Оля=u0020Зимой.mp3"
      ],
      "8350861f98b17d5d65585f4723763575c5662dd378a4d710d74c87b0d9c5002e38a829e84ac39bc1c10f955db35a3ddf88a71a062538f7c1fb67183e4b4d6f00": [
         "v1/content/data/vector/ki.ai"
      ],
      "87c82e1a5b55190ae102de694da2cd0e2e87d7b4446017141d671d6cf9e270d6d3909700c3d7b74c8e1a68f7c20a2206e0779ecc054a222363c899cfa31c5644": [
         "v1/content/metadata/info.json"
      ],
      "99f845e2dd5cd4f0dbcc50d69eea15cc4f6b70891dae66131626f974a9bfb2774bbebb9b1b94cda31717fccc31e783ac4684657d31e41ad6f8d4b473011a5f74": [
         "v1/content/data/vector/ki.svgz"
      ],
      "9c9d3b13a35cd946f8def38e1f6bec1704818728d5232fb5d6abde8b9657646f7861a810375bd506bc6c858dac830d64a8e109bab89f6f4a4423adc6b7e35259": [
         "v1/content/data/audio/Education=u0020of=u0020the=u0020Noobz=u00201.docx"
      ],
      "9ccf2683245962815e3701acd06ac9db446d57e11841e1e11b0359e7d4986a7d19d03dd302b434b42dc45518cef6a38d82d0943e449f536d28c61fed5eb015d9": [
         "v1/content/data/vector/ki.eps"
      ],
      "a48137dd54a343c5c9cd21ae0bc2bdbef4b383d9b71b0cb11fc81735a87782fba8a490a425a19ec914299e3525850b368ad850b09788231c45771189bfcbd8d5": [
         "v1/content/data/vector/ki.svg"
      ],
      "aadf1cb1c59a57d892900a905daa841a01c81dca9d09f8d572cc6dd0a20df2a2c4c8182d292a0150948cd43a31bf6565ef99e58e45de03f3d8aa0b6fb48ca769": [
         "v1/content/data/audio/Education=u0020of=u0020the=u0020Noobz=u00201.odt"
      ],
      "ac35fb4998da00ee0ad957988976bacc2c70a5c2c9622c3d7ac81c95cb1308a569fc6bb19d9f965614fb4d1376cd33193b83b1b65ef4795eecbfcd5ac254fc61": [
         "v1/content/data/text/Testseite=u0020-=u0020Kopie.pdf",
         "v1/content/data/text/Testseite.pdf"
      ],
      "b37e496407df134d822a35a3fee4b7fad3c3d3fe94d33ee31a5982bdeb978751143d5a490e1e5ff3aa9f9f94a6d3e5558e75ba96a2d1486994973cceea1ee654": [
         "v1/content/data/video/Together=u0020Elsewhere=u002001.docx"
      ],
      "b78834c6ee794093dcde31cb8b1680351f12f3c61043b4e202fb0bfc03db235bfa2d5133bba202bb12892313af55c21826e4b7cf5f4f6cd340dff95ed9607ef4": [
         "v1/content/data/image/IMG_6914.tga"
      ],
      "cf83e1357eefb8bdf1542850d66d8007d620e4050b5715dc83f4a921d36ce9ce47d0d13c5d85f2b0ff8318d2877eec2f63b931bd47417a81a538327af927da3e": [
         "v1/content/data/=u007Etest=u005B0=u005D/.empty"
      ],
      "d47b0e8f8e47d9c530e7d9b9efbcdd758e21b6bd80957c1853ade8e7da5c08050b90580dcf10d3e416938afc5e7f373457e4f7da3d6be37a77678065b2cd02ee": [
         "v1/content/data/text/fulltext_5693376.xml"
      ],
      "fff7d145da8e0a4f64b836cb6bdf51d998e384b590fd1815673fb951215f6d902a9f298e67ae6d56e43111b78cf825f4a5efbcdaa0b6e62c6c2317c03ab73609": [
         "v1/content/metadata/thumbnail_v1.jsonl.gz"
      ]
   },
   "versions": {
      "v1": {
         "created": "2026-03-15T15:04:33Z",
         "message": "initial commit",
         "state": {
            "0f45fc1448863daf9da4deeed7be5c30fa04e6e82a6d136662c642fcee49f22883795fe747d15e106f61a15e507c8aedb47b32022674763cba49e565f5274140": [
               "metadata/thumbnails/v1/00003.png"
            ],
            "1082b5603213566c3d1d2a27146d10bf2f874253ec688a3e8736f40e15a356d3ffdd50036417a246563bc195f4064f2431ab317a4a9bd64edd32d589b87bfcd4": [
               "data/image/IMG_6914.jpg"
            ],
            "18c77a1d494a0203a80798132ab8a961894bac15300dec868bc31cecd64c47c9c36e875061b6145c0ae66ce3ab0eb973217217080314475ff5eafd5da62f73dd": [
               "data/image/IMG_6914.png"
            ],
            "1bbb24b851ab64ff649ddd038b3597abe3156776b2c53ac92dc72c98e2699d05f2027e32cbd9f5c6b6484a46c4012fbf1a383d0dd8840686878477f72cd2789c": [
               "metadata/indexer_v1.jsonl.gz"
            ],
            "228fbd448ccadaa1c7a7b8c6d7bab5bcb9bea862f21317b16f1968abd1cfda97d9833fc1b91f4d77ee0dab52df02576d469a748219f62dd4c49af99aa5f995e2": [
               "data/video/Together Elsewhere 01.pdf"
            ],
            "2a078ec51388d5e1c63c490b994b173c2853716afa9a478bc7b234cfa0c839953873b0d19ffa8a264e3bf4230c580ae2db02215189957fd65a0e1ffb508b7f5b": [
               "metadata/thumbnails/v1/00002.png"
            ],
            "394e8422bd570224994ff6dabf2a0b7f5e8b9f7817f08ccf42ca0856a3a072e5bc852668f17f69f2c15a75251f03bdcf03a70af280d8699ea6eadb98703c7337": [
               "metadata/filesystem_v1.jsonl.gz"
            ],
            "4d3f34495b648372005f544f9cca3d3702fa29b1d89f6a2864d273c37c96cd3ed1773c8b582d6815cedb1a9c89f0a24623ae4ee0608dc1acc4f3f5837e8fbb22": [
               "metadata/thumbnails/v1/00010.png"
            ],
            "55d70662e2da382a0a1113b7fa77bb910b50b53f61f1af994af1b09a3bd3069e1a1bc6c975f27e93dbffaf8fc70a67f1bc3e599f2039c468b5689bb32d85a4a0": [
               "metadata/thumbnails/v1/00004.png"
            ],
            "56f08a903abf5b2a9feb6bebab95b4dbaad75d04727638cdaad8e172c1245d84efb9a0c99abcd937d2527b3a26452ba57fda9172fc24c012c874522b893ab2b7": [
               "data/image/IMG_6914.gif"
            ],
            "5bf773add0abe51fc39f1054ab2e34fceae987e431a8b97c3780d0c0948132cde33c2d7d98b8b94dd49daaf15f564ad2a4663a7181b5f8231e37576fff4a1e67": [
               "data/audio/Education of the Noobz 1.doc"
            ],
            "6d232473dca41be83138d720c2b5a600c6ec05f5ec46dab04aee93cc2d8834dcc3e35811c484320ca08d5d1554d87a6ec8afd07fb809708dc0c54c3ad473be6b": [
               "data/video/together_01_excerpt.mkv"
            ],
            "6f6a6418e72610c0021412627ac89a5d1acb3c90a6fdef7d8c408c96bdbaa2f528b307f99fb72dd83458d796a10601ce012d7749d4b4493c163cd09ec5ce9b89": [
               "README.md"
            ],
            "72b182ad228d3092cd3a8fadbb75d739d55fd938f0557d149720bb0b32ae48604b9b8e2f0b0112d5ce67aa6cff512a944eafc90ea5e41570902ec7f2b757584d": [
               "metadata/thumbnails/v1/00001.png"
            ],
            "763712ca6ecf1959287c84a29c43f76e89ab83ab3bd901867450ab77e737ee72cdaa3d0a8f4a8224e61736efa6fa5a6b754ef40229f1500edc4b11215d3b3ee9": [
               "data/vector/ki.pdf"
            ],
            "7841cd462db95b5354012b866f599bf75540cd7edc904b108cf0e933d79c9b4e6481c511fd2a1506d8223732897cc151735b2b16755db81c6f124ff4fc1ccd28": [
               "data/text/export_mets.xml"
            ],
            "791e4839906d0303cbfe97e3f092b5b375210fb9ca25f88e75a937a28e93b16a17ae58c87a79f2ed5d92b6405f29ab5b119d808c598111642d3e1e31c700b182": [
               "data/image/IMG_6914.webp"
            ],
            "79f7273c26222b0b843d19495e9c6c31492bf0d7df4e5d161b8d77693504e763698b9f093f1c889f8c85053fecc8dea6bfdfffcdda894c6c3476448543477735": [
               "data/audio/Dragan_Espenschied_-_10_-_Оля Зимой.mp3"
            ],
            "8350861f98b17d5d65585f4723763575c5662dd378a4d710d74c87b0d9c5002e38a829e84ac39bc1c10f955db35a3ddf88a71a062538f7c1fb67183e4b4d6f00": [
               "data/vector/ki.ai"
            ],
            "87c82e1a5b55190ae102de694da2cd0e2e87d7b4446017141d671d6cf9e270d6d3909700c3d7b74c8e1a68f7c20a2206e0779ecc054a222363c899cfa31c5644": [
               "metadata/info.json"
            ],
            "99f845e2dd5cd4f0dbcc50d69eea15cc4f6b70891dae66131626f974a9bfb2774bbebb9b1b94cda31717fccc31e783ac4684657d31e41ad6f8d4b473011a5f74": [
               "data/vector/ki.svgz"
            ],
            "9c9d3b13a35cd946f8def38e1f6bec1704818728d5232fb5d6abde8b9657646f7861a810375bd506bc6c858dac830d64a8e109bab89f6f4a4423adc6b7e35259": [
               "data/audio/Education of the Noobz 1.docx"
            ],
            "9ccf2683245962815e3701acd06ac9db446d57e11841e1e11b0359e7d4986a7d19d03dd302b434b42dc45518cef6a38d82d0943e449f536d28c61fed5eb015d9": [
               "data/vector/ki.eps"
            ],
            "a48137dd54a343c5c9cd21ae0bc2bdbef4b383d9b71b0cb11fc81735a87782fba8a490a425a19ec914299e3525850b368ad850b09788231c45771189bfcbd8d5": [
               "data/vector/ki.svg"
            ],
            "aadf1cb1c59a57d892900a905daa841a01c81dca9d09f8d572cc6dd0a20df2a2c4c8182d292a0150948cd43a31bf6565ef99e58e45de03f3d8aa0b6fb48ca769": [
               "data/audio/Education of the Noobz 1.odt"
            ],
            "ac35fb4998da00ee0ad957988976bacc2c70a5c2c9622c3d7ac81c95cb1308a569fc6bb19d9f965614fb4d1376cd33193b83b1b65ef4795eecbfcd5ac254fc61": [
               "data/text/Testseite - Kopie.pdf",
               "data/text/Testseite.pdf"
            ],
            "b37e496407df134d822a35a3fee4b7fad3c3d3fe94d33ee31a5982bdeb978751143d5a490e1e5ff3aa9f9f94a6d3e5558e75ba96a2d1486994973cceea1ee654": [
               "data/video/Together Elsewhere 01.docx"
            ],
            "b78834c6ee794093dcde31cb8b1680351f12f3c61043b4e202fb0bfc03db235bfa2d5133bba202bb12892313af55c21826e4b7cf5f4f6cd340dff95ed9607ef4": [
               "data/image/IMG_6914.tga"
            ],
            "cf83e1357eefb8bdf1542850d66d8007d620e4050b5715dc83f4a921d36ce9ce47d0d13c5d85f2b0ff8318d2877eec2f63b931bd47417a81a538327af927da3e": [
               "data/~test[0]/.empty"
            ],
            "d47b0e8f8e47d9c530e7d9b9efbcdd758e21b6bd80957c1853ade8e7da5c08050b90580dcf10d3e416938afc5e7f373457e4f7da3d6be37a77678065b2cd02ee": [
               "data/text/fulltext_5693376.xml"
            ],
            "fff7d145da8e0a4f64b836cb6bdf51d998e384b590fd1815673fb951215f6d902a9f298e67ae6d56e43111b78cf825f4a5efbcdaa0b6e62c6c2317c03ab73609": [
               "metadata/thumbnail_v1.jsonl.gz"
            ]
         },
         "user": {
            "address": "mailto:ocfl.user@unibas.ch",
            "name": "User OCFL"
         }
      }
   },
   "fixity": {
      "blake2b-384": {
         "025847c1e510f9f9ffd7f96f81f4039429f77d9aae3c9e80cba756c812fde938890febca1b0a2c2977ad7da7399417f3": [
            "v1/content/metadata/info.json"
         ],
         "2c36366029ba07257edc1b287420d22fc327637cfba3cbe60ea6236eb92ad59d2a30b8bf13867a56aebf0016b56c8d37": [
            "v1/content/data/audio/Education=u0020of=u0020the=u0020Noobz=u00201.odt"
         ],
         "30729eb1679800364e5d626ab677db1d0f82a2bde7b65c1966ad575115876bc3ed9fb4951110b7d431c0b39911a939b7": [
            "v1/content/data/image/IMG_6914.gif"
         ],
         "3582e912e396f3f4f004c472ecc3e50be44af37a120ad1eb6e855a2279c6479e6609e64476979afed06a4d14357f7378": [
            "v1/content/metadata/indexer_v1.jsonl.gz"
         ],
         "40944cb1cce86a98e340f4c3a29fbc8d10ad26e6cfaf085cfe329740ad8dacd293c99092db3c0031ddf10cd3465373ab": [
            "v1/content/data/audio/Dragan_Espenschied_-_10_-_Оля=u0020Зимой.mp3"
         ],
         "4125de5814d9fcbd9fc0d693f02ad9f2d1a9d019156d1910858860b65735014e956b2f05804e4bbbdffb81090c39cd8e": [
            "v1/content/data/vector/ki.pdf"
         ],
         "42c5d985cb2183cae43bbd9494f834001fe8ef6529bff401aec8bb145a7d4f4d3788ea1c7390cc00a5617a4c5ea9d36d": [
            "v1/content/metadata/thumbnails/v1/00004.png"
         ],
         "44d526866f281c8650f21dc9b09544898e9bf38f02fa56727bc8a4d2767d68a71c9984f9ac021f9df9617418fe3e59f6": [
            "v1/content/data/vector/ki.ai"
         ],
         "47049d6d36fa6bc6926b02c06f42f22696a248d879091cef3b12211429625263dcc28570508ed0e2025d7c93d6b53b38": [
            "v1/content/data/text/fulltext_5693376.xml"
         ],
         "4bf93163830a4008ea226279a3a9f9cc391f63389c3c0832204db954ecdb871df5791a1f4d36aad2a6df2a310e2fbdcf": [
            "v1/content/data/audio/Education=u0020of=u0020the=u0020Noobz=u00201.docx"
         ],
         "5866b7e75f8d1454a41608c95049e91b0e75b9236c4ee51a65f2d1bd4f2c901dd8339edb7f6572a1ed7ee733f587ec26": [
            "v1/content/data/audio/Education=u0020of=u0020the=u0020Noobz=u00201.doc"
         ],
         "59c39bdd23db22650efdafe7a4778fb85e283cfe83c1ab386b5044d6781664f0a401460e3a069c793e2be26e4be34940": [
            "v1/content/data/image/IMG_6914.webp"
         ],
         "5d3785a16e89df264d813dd5285431528a4212b59c5a97129aaf79e9c976756178a7125f4c1662fda07578bf58a0dbe6": [
            "v1/content/metadata/thumbnails/v1/00001.png"
         ],
         "650dee6e7a81102a2470223a3e2f6d1b8f79db78f748e0d2e043e22cd40f8337b8c2ee7f7dd462b53274bd8d1ff1c716": [
            "v1/content/data/video/together_01_excerpt.mkv"
         ],
         "7689e3cc26d22ee2d188ed1792ebc7ded27eddfdd61209700f7a3fa21ef1eae2d499c0b01e47ced416f2a1c2fdb3ad74": [
            "v1/content/data/video/Together=u0020Elsewhere=u002001.pdf"
         ],
         "8b703dd9d745a1b6bd8497cead6056cb0e3e8c06632f3e2b2fef492e99ec26a9aedd1fc536a479607c7632e2c2cdbf6e": [
            "v1/content/metadata/thumbnails/v1/00010.png"
         ],
         "94063eb634b731808c02ab354dfafab395956fe9129a9ddc36a9c829956a55e90059a3ab0cf796b0caf40a9fb9ff03e5": [
            "v1/content/data/vector/ki.svg"
         ],
         "a3ea374ca4db4dd16fc2c62db484af041bf1eae8e9f11ca668580058d213a4fb90f39258e74bcf35f8ebf3c52e1125a2": [
            "v1/content/README.md"
         ],
         "ab150f1101fea53e5dec9eda3dd993e5cb86acd58a86115a3ece1394607a0253ccc1e2e7e75abf115f302cb547e34198": [
            "v1/content/metadata/filesystem_v1.jsonl.gz"
         ],
         "afb90b170b9246dd5a641eb25e24a6716d193dd5e7a5ad5364efd04227e77d753c82b7b7a59eb9124a264150717d0e05": [
            "v1/content/metadata/thumbnail_v1.jsonl.gz"
         ],
         "b32811423377f52d7862286ee1a72ee540524380fda1724a6f25d7978c6fd3244a6caf0498812673c5e05ef583825100": [
            "v1/content/data/=u007Etest=u005B0=u005D/.empty"
         ],
         "bab772b98920eb1607f61a0dd055cf1f2b2755109cff109cdf4a53c2a0407c5c8d4ac0fe451b1a219c9fed41f10e236b": [
            "v1/content/data/text/export_mets.xml"
         ],
         "c393e97b964488a54b698c8d1adf3d3831a8deaea4c0932fcb314aa94e4b1a22647d6a327447f59e576f8a57e79e1a1e": [
            "v1/content/data/text/Testseite=u0020-=u0020Kopie.pdf",
            "v1/content/data/text/Testseite.pdf"
         ],
         "cd756f2086581d5683c627309a91dc081dee8b76a0f0e82d8d68093ca923b1cda55f5507ba58f0c840121077cb094fae": [
            "v1/content/data/image/IMG_6914.tga"
         ],
         "ce019613eda598db3b8e955a8f43046e256f65a57df168095ec3fdcb200384257be79611a6766eac01ff26fcf5d11c2a": [
            "v1/content/metadata/thumbnails/v1/00003.png"
         ],
         "d556cbc39fce2ef956a476bee3eed716e9cdc5c57931cfe63dd15530629a5e3edc39d6f37e2dd9c3426e57d15f03603b": [
            "v1/content/data/video/Together=u0020Elsewhere=u002001.docx"
         ],
         "d84cf73db9098338ffc624098fa10d31d2f2e6a971b65e4f4699ab7709969d492a35364266ce31cd3a73a87d1975fd4f": [
            "v1/content/data/image/IMG_6914.png"
         ],
         "f223ad3a7adacfdde401e7117c37aecf94206dc54c3a9b6a9d950429fa609cb2495984361c7142302e091779b81d69e2": [
            "v1/content/data/vector/ki.eps"
         ],
         "f3035026fc7860fa2c55e13543cb71e7ce40c92dfcc85044a613a53de04462132d1f72ec72232762b690ecdb6193211b": [
            "v1/content/data/image/IMG_6914.jpg"
         ],
         "f828ac9fa635e21a60d41d26098c6b95f566cc9b9019d0f43d5efb790109cd643196e103c295c117e0db761cb4303cfd": [
            "v1/content/metadata/thumbnails/v1/00002.png"
         ],
         "fda4a5c45ebb9353ec52aa25cd1652df0edcc2191a4d2efc61e7fae887f8297e79457d48413aeb7b501045c6214b461e": [
            "v1/content/data/vector/ki.svgz"
         ]
      }
   }
}
```

## Explanation of Key Areas

### 1. Header Information
- **`id`**: The unique identifier of the object (`urn:nbn:de:gbv:42-test1`).
- **`type`**: Reference to the OCFL specification used (here 1.1).
- **`digestAlgorithm`**: The primary hash algorithm (default: `sha512`). It is used for the manifest and state descriptions.
- **`head`**: The most recent version of the object (here `v1`).
- **`contentDirectory`**: The name of the directory within each version that contains the physical files (default: `content`).

### 2. Manifest
The `manifest` is a flat list of all physical files present in the object. Each file is referenced by its hash value. The hash is the key, and the value is an array of paths (since OCFL supports deduplication, one hash could refer to multiple paths).

### 3. Versions
This is where the history of the object is represented. Each version (e.g., `v1`) contains:
- **`created`**: Timestamp of creation.
- **`message`**: Description of the change.
- **`user`**: Who made the change.
- **`state`**: This is the **logical state** of the version. It maps the original directory structure to the hashes in the manifest. Here you see the "clean" names (e.g., with spaces), while the manifest contains the physical paths (with URL encoding).

### 4. Fixity
The `fixity` block is optional and contains alternative hashes (like `md5`, `sha1`, or `blake2b-384`) for the files listed in the manifest. This serves the purpose of long-term integrity checking using various algorithms.

---

[Back to Object Structure](../05_object_structure_en) | [Back to Table of Contents](../toc_en) | [Next Topic: Updating Objects](../06_update_object_en)
