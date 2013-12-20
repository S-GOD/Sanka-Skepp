Sanka-Skepp
===========
package 
{
	import flash.display.SpreadMethod;
	import flash.display.Sprite;
	import flash.events.Event;
	import flash.events.KeyboardEvent;
	import flash.events.MouseEvent;
	import flash.text.TextField;
	
	/**
	 * ...
	 * @author Adam Sjödahl
	 */
	public class Main extends Sprite 
	{
		public var map:Vector.<Vector.<Vatten>> = new Vector.<Vector.<Vatten>>();
		private var hitOrMissText:TextField;
		private var hitOrMiss:Number;
		private var text:TextField = new TextField();
		
		
		public function Main():void 
		{
			if (stage) init();
			else addEventListener(Event.ADDED_TO_STAGE, init);
		}
		
		private function init(e:Event = null):void 
		{
			removeEventListener(Event.ADDED_TO_STAGE, init);
			// entry point
			
			text.textColor = (0x000080);
			text.border = true;
			text.autoSize;
			text.borderColor = (0x00FF00);
			text.text = "Press space to start";
			text.x = 300;
			text.y = 300;
			addChild(text);
			
			hitOrMiss = 0;
			hitOrMissText = new TextField;
			hitOrMissText.x = 50;
			addChild(hitOrMissText);
			updateHitMiss();
			
			stage.addEventListener(KeyboardEvent.KEY_DOWN, spaceReset);
		}
		private function newMap():void 
		{
			hitOrMiss = 0;
			hitOrMissText = new TextField;
			hitOrMissText.x = 50;
			addChild(hitOrMissText);
			updateHitMiss();
			
			for (var i:int = 0; i < 10; i++) 
			{
				var enRad:Vector.<Vatten> = new Vector.<Vatten>();
				for (var j:int = 0; j < 10; j++) 
				{
					var t:Vatten = new Vatten();
					t.y = 100 + j * (Vatten.TILE_SIDE + 1);
					t.x = 100 + i * (Vatten.TILE_SIDE + 1);
					this.addChild(t);
					this.addEventListener(MouseEvent.CLICK, clickOnBox);
					enRad.push(t);
					
				}
				map.push(enRad);
				
 			}
		}
		
		private function spaceReset(e:KeyboardEvent):void 
		{
			if (e.keyCode == 32) 
			{	
				//Reset allt på mappen
				
				while (numChildren > 0) 
				{
					removeChildAt(0);
				}
				// töm vectorn
				map.length = 0;
				//Skapar nytt spel
				newMap();
				//Scoren resetas
				hitOrMiss = 0;
				hitOrMissText.text = "Score: 0"
				
				PlaceShip(3);
				PlaceShip(3);
				PlaceShip(3);
				
	
			}
		}
		private function clickOnBox(e:MouseEvent):void 
		{
			Vatten(e.target).clicked();
			
			if (e.target == 1) 
			{
				hitOrMiss++;
				updateHitMiss();
			}
		}
		private function updateHitMiss():void 
		{
			hitOrMissText.text = "Score: " + hitOrMiss;
		}
		private function PlaceShip(shipLength:int = 3):void
		{
			var isHorizontal:Boolean = false;
			var skeppX:int;
			var skeppY:int;
			var tempShip:Vector.<Vatten> = new Vector.<Vatten>();
			if (Math.random() > 0.5)
			{
				isHorizontal = true;
			}
			if (isHorizontal)
			{
				skepPX = Math.random() * (map.length-shipLength);
				skeppY = Math.random() * (map.length - 1);
				for (var i:int = 0; i < shipLength; i++) 
				{
					tempShip.push(map[shipX+i][shipY]);
				}
			}
			else
			{
				skeppX = Math.random() * (map.length - 1);
				skeppY = Math.random() * (map.length-shipLength);
				for (var i:int = 0; i < shipLength; i++) 
				{
 					tempShip.push(map[shipX][shipY+i]);
				}
			}
			for (var j:int = 0; j < tempShip.length; j++) 
			{
				if (tempShip[j].isShip)
				{
					PlaceShip(shipLength);
					return;
				}
			}
			for (var k:int = 0; k < tempShip.length; k++) 
			{
				tempShip[k].isShip = true;
			}
		}
	}
	
