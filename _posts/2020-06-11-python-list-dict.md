---
layout: post
title: python 的 遞迴 list 與 dict
date: 2020-06-11
tags: python dict list
---

下面，我們提供了一個名為big_list的嵌套列表。 使用嵌套迭代創建一個字典word_counts，該字典包含big_list中的所有單詞作為鍵，並包含它們作為值出現的次數。

```
big_list = [[['one', 'two'], ['seven', 'eight']], [['nine', 'four'], ['three', 'one']], [['two', 'eight'], ['seven', 'four']], [['five', 'one'], ['four', 'two']], [['six', 'eight'], ['two', 'seven']], [['three', 'five'], ['one', 'six']], [['nine', 'eight'], ['five', 'four']], [['six', 'three'], ['four', 'seven']]]

def check(l):
    m =[]
    for i in l:
        if isinstance(i,list):
            m.extend(check(i)) # 兩個 list 合併
        else:
            m.append(i)
    return m

get_list=check(big_list)
word_counts = {}
for i in get_list:
    if i not in word_counts:
        word_counts[i] = 0
    word_counts[i] = word_counts[i] + 1
```

提供的詞典包含口袋妖怪圍棋玩家數據，每個玩家在其中揭示每個口袋妖怪所擁有的糖果量。 如果將所有數據匯總在一起，哪個口袋妖怪的糖果數量最多？ 將該寵物小精靈分配給變量most_common_pokemon.

```
pokemon_go_data = {'bentspoon':
                  {'Rattata': 203, 'Pidgey': 120, 'Drowzee': 89, 'Squirtle': 35, 'Pikachu': 3, 'Eevee': 34, 'Magikarp': 300, 'Paras': 38},
                  'Laurne':
                  {'Pidgey': 169, 'Rattata': 245, 'Squirtle': 9, 'Caterpie': 38, 'Weedle': 97, 'Pikachu': 6, 'Nidoran': 44, 'Clefairy': 15, 'Zubat': 79, 'Dratini': 4},
                  'picklejarlid':
                  {'Rattata': 32, 'Drowzee': 15, 'Nidoran': 4, 'Bulbasaur': 3, 'Pidgey': 56, 'Weedle': 21, 'Oddish': 18, 'Magmar': 6, 'Spearow': 14},
                  'professoroak':
                  {'Charmander': 11, 'Ponyta': 9, 'Rattata': 107, 'Belsprout': 29, 'Seel': 19, 'Pidgey': 93, 'Shellder': 43, 'Drowzee': 245, 'Tauros': 18, 'Lapras': 18}}

def check(d):
    m={}
    for k, v in d.items():
        if isinstance(v,dict):
            m = { k: m.get(k, 0)+check(v).get(k, 0) for k in set(m) | set(check(v)) } # 兩個 dict 合併 且將相同的 KEY 其值相加
        else:
            m[k]=v
    return m

data = check(pokemon_go_data)
for i in sorted(data , key=data.get):
    most_common_pokemon = i
```
