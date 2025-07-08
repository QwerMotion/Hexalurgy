![image](https://github.com/user-attachments/assets/a26af0ad-4610-4eec-a90e-5ee1adedffe0)


![image](https://github.com/user-attachments/assets/e7467f7d-092d-415f-b67f-4432f4222e7d)
08.07.2025 inventar


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

Das Inventar (07.07.2025)
---------------

Das Inventar ist ein Node2d und ein Kind vom Spieler-Node. Es ist außerdem eine Klasse namens PlayerInventory. Es hat als Kind-Knoten einen ControlNode namens player_inventory_ui, was sich um die Anzeige des Inventars kümmert. 
PlayerInventory hat ein Attribut, welches InventoryData heißt. InventoryData ist wiederum eine Klasse, die ein Array[ItemInstance] verwaltet, und methoden fürs hinzufügen und entfernen von ItemInstances anbietet und theoretisch auch fürs speichern und laden (persistent). Für das laden und speichern muss allerdings die ItemInstance klasse nochmal erweitert werden, damit sie eine to_dictionary methode anbietet. Das Inventar ruft momentan jeden frame update_inventory(data : InventoryData) auf. Der Ui-Knoten geht dann durch alle seine slots und weist jedem das korrospondierende ItemInstance zu. Die Slots haben dann das Sprite des Items und zeigen als label den count an. Sie haben bei der erstellung außerdem einen Index bekommen, den sie zurückgeben, wenn man sie anklickt (es sind texture-buttons). Momentan ist es nicht gedacht, dass sich die größe des Inventars zur laufzeit ändert. Es ist außerdem wichtig, dass das item_array von InventoryData genauso lang ist, wie das player_inventory_ui slots hat, ansonsten kommt es zu out of bounds fehlern. Außerdem ist es so, dass die item_pickups jeweils eine ItemInstance halten, welche theoretisch auch einen count haben könnte, der ungleich 1 ist. (also z.B. es wird ein kupfer_ore_block abgebaut, welcher dann copper_ore_item : ItemInstance droppt, mit einem count von 4 und einem type : ItemType von copper_ore), das sollte eigentlich, wenn das inventar z.B. nur noch 1 erz fassen kann, dazu führen, dass das eine erz hinzugefügt wird, und das pickup den rest behält, das ist aber NICHT GETESTET. Wenn 4 erze droppen sollen, sind 4 pickups zu erzeugen, die jeweils 1 erz darstellen, das ist halbwegs getestet. 

Das Inventar, Teil 2 (08.07.2025)
---------------

Das Inventar hat jetzt eine große von size (aktuell 51 slots) der letzte slot (mit dem index size - 1, also 50) ist dabei der curser-slot. Wenn die slots in den grid_containern angeordnet werden, wird der letzte zum curser_slot, indem einige attribute gesetzt werden, die dann in _process() dafür sorgen, dass der curser_slot immer dem Mauszeiger folgt. Dieser letzte slot ist dabei kein kind vom grid_container, sondern vom player_inventory_ui. Aber, der node ist dennoch im slots : Array[InventorySlot] array. (ist also ein normales InventorySlot, was auch von update_inventory erfasst wird.) Das hat den effekt, dass man, wenn man beispielsweise gneis_block_item im curser_slot hat und der count bei 100 liegt und der max_stack_stack_size bei 128, und wenn man dann weitere gneis_block_items aufgesammelt werden, wird das ganze auch bei geschlossenem inventar aktualisiert. Es gibt aktuell beim swappen von einem slot auf den curser_slot noch keine stack-funktionalität, es wird also immer getauscht. Es gibt weiterhin noch keine möglichkeit 1 item "vom stapel zu nehmen" nur den ganzen stack. Ein item nehmen wird realisiert wenn z.B. ein Rechtsklick auf einem slot bemerkt wird. Dann wird, vom slot das item_to_hold.count um 1 reduziert und das auf dem curser_slot um 1 erhöht, falls sie kompatibel sind oder falls der curser_slot leer ist. Wenn das item_to_hold.count auf 0 fällt, muss das item_to_hold auf null gesetzt werden. Ist der curser voll und der slot leer soll 1 auf den slot übertragen werden. Sind beide voll und nicht kompatibel soll einfach gar nichts passieren. 

Nächste Schritte
----------------

Ein Globales Script einfügen, was es ermöglicht mit einer Hashmap anhand der ID des BlockTypes den tatsächlichen BlockType zu bekommen. Damit sollte es dann kein Problem sein, wenn das BlockItem genutzt wird den entsprechenden Block zu platzieren



Inventar weiter bauen, aktives item merken, items aus dem inventar benutzen, item interpreter im spieler script
