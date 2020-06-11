### Damage

[Damage formula](https://blogs.nrvnqsr.com/entry.php/3309-How-is-damage-calculated):

```
damage for this card =
(01)    servantAtk
(02)    * npDamageMultiplier
(03)    * {firstCardBonus + [cardDamageValue * (1 + cardMod)]}
(04)    * classAtkBonus
(05)    * triangleModifier
(06)    * attributeModifier
(07)    * randomModifier
(08)    * 0.23
(09)    * (1 + atkMod - defMod)
(10)    * criticalModifier
(11)    * extraCardModifier
(12)    * (1 - specialDefMod)
(13)    * [1 + powerMod + selfDamageMod + (critDamageMod * isCrit) + (npDamageMod * isNP)]
(14)    * [1 + ((superEffectiveModifier - 1) * isSuperEffective)]
(15)    + dmgPlusAdd
(16)    + selfDmgCutAdd
(17)    + (servantAtk * busterChainMod)
```

Mapping of damage formula terms to [buff actions](https://api.atlasacademy.io/export/JP/NiceBuffList.ActionList.json):

* cardMod = actor.commandAtk - target.commandDef
* atkMod = actor.atk
* defMod = target.def
* specialDefMod = target.specialdefence
* powerMod = actor.damage + actor.damageIndividuality + actor.damageIndividualityActiveonly + actor.damageEventPoint
* selfDamageMod = target.selfdamage
* critDamageMod = actor.criticalDamage
* npDamageMod = actor.npdamage
* superEffectiveModifier = function Correction value
* dmgPlusAdd = actor.givenDamage
* selfDmgCutAdd = target.receiveDamage

Other constants lookup:

* firstCardBonus, cardDamageValue:
  * firstCardBonus: [NiceCard](https://api.atlasacademy.io/export/JP/NiceCard.json).card.order.addAtk
  * cardDamageValue: [NiceCard](https://api.atlasacademy.io/export/JP/NiceCard.json).card.order.adjustAtk
* classAtkBonus: [NiceClassAttackRate](https://api.atlasacademy.io/export/JP/NiceClassAttackRate.json).class
* triangleModifier: [NiceClassRelation](https://api.atlasacademy.io/export/JP/NiceClassRelation.json).actor.target
* attributeModifier: [NiceAttributeRelation](https://api.atlasacademy.io/export/JP/NiceAttributeRelation.json).actor.target
* criticalModifier: [NiceConstant](https://api.atlasacademy.io/export/JP/NiceConstant.json).CRITICAL_ATTACK_RATE = 2
* extraCardModifier:
  * [NiceConstant](https://api.atlasacademy.io/export/JP/NiceConstant.json).EXTRA_ATTACK_RATE_GRAND = 3.5 if it's a color brave chain
  * [NiceConstant](https://api.atlasacademy.io/export/JP/NiceConstant.json).EXTRA_ATTACK_RATE_SINGLE = 2 otherwise (only applies to extra card)