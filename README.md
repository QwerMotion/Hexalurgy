# Hexalurgy
tech-heavy terraria with hexagons


*Wie blöcke funktionieren*
--------------------------
Der Welt-Manager hat eine tilemaplayer, welche die Welt visualisiert.
Außerdem gibt es bisher eine hashmap namens block_layer{} die als schküssel Vec2i's benutz und als Werte *BlockInstances*.


Was *BlockInstances* sind
--------------------------

BlockInstances ist eine Klasse, die als Attribute einmal Werte hat, die den Status des Blockes wiedergeben und die zwischen jedem BlockInstance varriieren können (aktuelle hp beispielsweise, kann bei verschiedenen Gneis-Blöcken unterschiedlich sein), und zum anderen ein *BlockType*-Objekt. *BlockType* ist wiederum eine Klasse, (eine ressource mit dem class_name BlockType) die die statischen eigenschaften einer Blockart definiert. 
Daszu gehört welches Geräusch abgespielt wird - beim beschädigen, beim zerstören - was die max_hp und die härte des Blockes ist. 
