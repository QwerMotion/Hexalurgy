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

Das Inventar
---------------

Das Inventar ist ein Node2d und ein Kind vom Spieler-Node. Es ist außerdem eine Klasse namens PlayerInventory. Es hat als Kind-Knoten einen ControlNode namens player_inventory_ui, was sich um die Anzeige des Inventars kümmert. 
PlayerInventory hat ein Attribut, welches InventoryData heißt. InventoryData ist wiederum eine Klasse, die ein Array[ItemInstance] verwaltet, und methoden fürs hinzufügen und entfernen von ItemInstances anbietet und theoretisch auch fürs speichern und laden (persistent). Für das laden und speichern muss allerdings die ItemInstance klasse nochmal erweitert werden, damit sie eine to_dictionary methode anbietet. Das Inventar ruft momentan jeden frame update_inventory(data : InventoryData) auf. Der Ui-Knoten geht dann durch alle seine slots und weist jedem das korrospondierende ItemInstance zu. Die Slots haben dann das Sprite des Items und zeigen als label den count an. Sie haben bei der erstellung außerdem einen Index bekommen, den sie zurückgeben, wenn man sie anklickt (es sind texture-buttons). Momentan ist es nicht gedacht, dass sich die größe des Inventars zur laufzeit ändert. 

Nächste Schritte
----------------

Ein Globales Script einfügen, was es ermöglicht mit einer Hashmap anhand der ID des BlockTypes den tatsächlichen BlockType zu bekommen. Damit sollte es dann kein Problem sein, wenn das BlockItem genutzt wird den entsprechenden Block zu platzieren

Inventar bauen
