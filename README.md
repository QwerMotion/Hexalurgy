![image](https://github.com/user-attachments/assets/a26af0ad-4610-4eec-a90e-5ee1adedffe0)



# Hexalurgy
tech-heavy terraria with hexagons


*Wie blöcke funktionieren*
--------------------------
Der Welt-Manager hat eine tilemaplayer, welche die Welt visualisiert.
Außerdem gibt es bisher eine hashmap namens block_layer{} die als schküssel Vec2i's benutz und als Werte *BlockInstances*. Darin werden die Blöcke gespeichert, für jeden Block in der welt gibt es hier den Eintrag.


Was *BlockInstance* ist und wodurch sich das von *BlockType* unterscheidet
--------------------------------------------------------------------------

BlockInstances ist eine Klasse, die als Attribute einmal Werte hat, die den Status des Blockes wiedergeben und die zwischen jedem BlockInstance-Objekt varriieren können (aktuelle hp beispielsweise, kann bei verschiedenen Gneis-Blöcken unterschiedlich sein), und zum anderen ein *BlockType*-Objekt. *BlockType* ist wiederum eine Klasse, (eine ressource mit dem class_name BlockType) die die statischen eigenschaften einer Blockart definiert. 
Daszu gehört welches Geräusch abgespielt wird - beim beschädigen, beim zerstören - was die max_hp und die härte des Blockes ist und welches *ItemType* in welcher quantität fallengelassen wird. 

Außerdem hat diese Klasse einen Konstrukor, der die hp auf die max_hp setzt und eine damage methode, da diese keinerlei kenntnis von außen voraussetzt. 

Was ist ein *ItemType*
----------------------

Wie bei den Blöcken setzen sich auch Items aus einer dynamischen, einzigartigen und einer statischen geteilten Komponente zusammen. *ItemInstance* ist dabei die Klasse, die beides vereint. Sie besitzt Attribute, die für gleiche Items variieren können, wie Haltbarkeit und count, sowie ein *ItemType*. Das ItemType ist dabei wieder eine Klasse (ressource mit class_name ItemType) die die statischen eigenschaften einer Item-Art definiert. Beispielsweise was die maximale stapelgrösse ist, was beim benutzen passieren soll usw. Aktuell enthält sie ein Attribut, was festlegt, welcher Block platziert werden soll, momental ist das ein String. 


Wie wird was gedroppt?
----------------------

Jeder BlockType definiert drops in ItemTypes und Quantitäten (Vec2i(min, max)). Der welt_manager (der auch die Methoden damage_block sowie player_damage_block) anbietet, schaut ob ein Block zerstört wird, falls ja und falls
der Block loot definiert hat, wird ein PickUp instanziiert mit dem richtigen sprite usw. Der Spieler soll es dann aufsammeln können, wodurch es zerstört wird, und der spieler das ItemInstance bekommt. 

Im Inventar, was wohl ein Array mit ItemInstances sein wird, muss dann geschaut werden, welche ItemInstances kompatibel sind (benutzt InstanceA das gleiche ItemType wie InstanceB...wie prüft man das, gibts sowas wie equal() in java?...)
und ob dann noch Platz auf dem Stapel ist. Items sollen ausserdem "rumschiebbar" sein, es gibt also sozusagen zwei zustände, die so eine Instanz im Inventar haben kann 1. In einem slot, 2. Im Curser (der wohl auch nur ein Slot ist) und es ergeben sich entsprechende transitionen a von curser zu slot, b von slot zu curser. Im falle a kann es sein, dass der slot leer ist a1 oder nicht a2. Ist der slot leer wird die Instanz vom Curser in den slot verschoben, ansonsten werden die items getauscht, falls sie inkompatibel sind a11 und die instanzen des cursers die auf den stapel des slots passen werden abgelegt wenn sie kompatibel sind a12. 

Nächste Schritte
----------------

Ein Globales Script einfügen, was es ermöglicht mit einer Hashmap anhand der ID des BlockTypes den tatsächlichen BlockType zu bekommen. Damit sollte es dann kein Problem sein, wenn das BlockItem genutzt wird den entsprechenden Block zu platzieren

Inventar bauen
