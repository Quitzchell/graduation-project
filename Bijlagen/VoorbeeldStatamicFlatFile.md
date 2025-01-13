# **Voorbeeld flat-file in Statamic**

```md
---
id: 78f550f5-b9a4-404a-86e5-fc684b3e3b77
blueprint: page
title: Home
header_title: "La gloire est éphémère, mais l'oubli est éternel."
header_image:
  - napoleon-on-horseback.jpg
blocks:
  -
    id: m4szm60u
    title: 'Bonjour à tous,'
    text: '<p>I am Napoleon Bonaparte, exiled here on the remote island of Saint Helena. Once, I commanded vast armies, reshaped nations, and navigated the turbulent waters of European politics. Today, however, I find myself in the serene isolation of this distant land, where the ocean whispers tales of glory and defeat.</p><p>As I pen my thoughts for you, dear readers, I invite you into my world—a realm of ambition, strategy, and, yes, introspection. Here, I shall share my reflections on leadership, the nature of power, and the lessons learned from both triumphs and trials.&lt;/p&gt;&lt;p&gt;Join me as I explore the intricate tapestry of history, the weight of legacy, and the fleeting nature of fame. Whether you seek inspiration, knowledge, or simply the musings of a man who once stood at the pinnacle of power, I welcome you to my journey.</p><p>À bientôt,<br>Napoleon</p>'
    namespace: common
    type: paragraph
    enabled: true
  -
    id: m4ua63d6
    title: 'A new conquest'
    text: '<p>I invite you to join me on a new conquest, not of nations but of cinema and literature! Much as I once sought to reshape Europe, I now seek to navigate the vast empire of film, and I need you by my side. Do you dare follow me, loyal subjects, in this new adventure? Then visit my reviews page into the world of film and literature!</p>'
    urlable_id: 8a254262-90b1-4eb7-918a-e7d60d81a683
    button_text: 'Read more'
    namespace: common
    type: call_to_action
    enabled: true
  -
    id: m4udna2o
    namespace: common
    type: map
    enabled: true
    google_maps:
      formatted_address: '129 Rue de Grenelle, 75007 Paris, France'
      coordinates:
        lat: 48.8581983
        lng: 2.3130361
    title: 'I await your visit'
    text: '<p>Visit me at Les Invalides, where I, Napoleon Bonaparte, rest. Stand before me, and discuss the ambition, triumphs, and sacrifices that shaped our history.</p>'
updated_by: 1
updated_at: 1734556580
template: homepage
---
```