# MCP Tips
Some useful tips for using in the minecraft code generated by mcp

## Getting started
Just configure your project here: [MCP-Reborn](https://github.com/Hexeption/MCP-Reborn)

## Newer Versions
#### I tried to use newer versions of mcp-reborn recently but I noticed that there were few to no tutorials or help on the internet. I just want to share my basic knowledge here for anyone who hasn't figured out some things in the newer versions


Before we get started: 
The `Minecraft` class is often the main entry point if you try to do somethig basic. I will use this class often here. 
You can get the instance with `Minecraft.getInstance()`.  
It's possible that some fields are private, just change them to public if needed.  

#### PoseStack:  
PoseStack is used by the new rendersystem, you will probably see it often and I personally didn't know how to get it or use it.  
Getting:   
In mose cases the poseStack is already in the method parameters, if so just use this. It is probably named like `p_92745_` or something.  
If it's not already passed, you can create a new Instance with `new PoseStack()` but avoid it at all cost.  
Usage:  
You can modify the objects with it, for example you can scale it etc.  
To use it call `stack.pushPose()` and if you finished `stack.popPose()`  

So lets get started.

- Drawing Text
```java
Minecraft.getInstance().font.draw(poseStack, text, x, y, color);
//Example:
Minecraft.getInstance().font.draw(poseStack, "Hello World", 100, height / 2, 0xFFFFFF); // would draw a Hello World in white at x position 100 and at the hight of the screen split in half
```

- Getting the scaled resolution
```java
//Width
Minecraft.getInstance().getWindow().getGuiScaledWidth()
//Height
Minecraft.getInstance().getWindow().getGuiScaledHeight()

//ScreenWidth
Minecraft.getInstance().getWindow().getScreenWidth()
//ScreenHeight
Minecraft.getInstance().getWindow().getScreenHeight()
```

- More things with getWindow()
```java
Window window = Minecraft.getInstance().getWindow();
//Set Title
window.setTitle("Minnekkraftt");
//Find best monitor
window.findBestMonitor();
//Toggle Fullscreen
window.toggleFullScreen();
//Toggle Windowed
window.setWindowed(int, int);

//Set screen width and height
window.setWidth(int) & window.setHeight(int)
```

- Get Player, Serverdata and World
```java
Minecraft.getInstance().player; //Player
Minecraft.getInstance().clientLevel; //World (Level)
Minecraft.getInstance().getCurrentServer() //Serverdata with onlineplayers, ip, version, motd, ping, icon, etc.
```

- Components  
To use text in some methods you need a Component, tho not the java Component.  
This is the import you need: `import net.minecraft.network.chat.Component;`  
To get an empty component: `Component.empty()`  
To get a text component: `Component.literal("TEXT")`  
To get a translatable component: `Component.translatable("menu.pause")`   
To get a keybind component: `Component.keybind()`; Usage: `Component.keybind(Minecraft.getInstance().options.keyKEY.getName())`  
To style a component: `Component.(...).withStyle(ChatFormatting.COLOR)`  

- Create Screen, add buttons, sliders and open screens  
Create a new Class, I would call it `YourguinameScreen` which extends Screen and implements/overrides some of the methods.   
The super constructor needs a component, explained above.  
In the `init` method you can initialize the screen, here is where you add your buttons etc.  
In the `render` method you get a PoseStack, mouseX, mouseY and partialTicks passed. Here is where you render text and draw the background or render images  
```java
import com.mojang.blaze3d.vertex.PoseStack;
import net.minecraft.client.gui.components.AbstractSliderButton;
import net.minecraft.client.gui.components.Button;
import net.minecraft.client.gui.screens.Screen;
import net.minecraft.client.gui.screens.inventory.InventoryScreen;
import net.minecraft.network.chat.Component;

public class TutorialScreen extends Screen {

    //Constructor, here you handle the fields etc.
    protected TutorialScreen() {
        super(Component.literal("My Screen"));
    }

    //Initializes the screen, add buttons here
    @Override
    protected void init() {
        //Add Button - 3rd and 4th arg is the size of the button
        addRenderableWidget(new Button(this.width / 2, this.height / 2, 98, 20, Component.literal("My Button"), (p_96323_) -> {
            //Do whatever the button should do
            System.out.println("Hello, I was clicked");
        }));

        //Add Slider - last argument is the max value
        addRenderableWidget(new AbstractSliderButton(this.width / 2, this.height / 2, 98, 20, Component.literal("My Button"), 1) {
            @Override
            protected void updateMessage() {
                //You got a value variable which is the current value of the slider, you can visualize it like that:
                setMessage(Component.literal("Value: " + value));
            }

            @Override
            protected void applyValue() {
                //This just applies the value, used for saving value or doing whatever the slider should do
            }
        });
    }

    @Override
    public boolean mouseClicked(double x, double y, int button) {
        return super.mouseClicked(x, y, button);
        //button == 0 -> leftclick
        //button == 1 -> rightclick
    }

    @Override
    public void render(PoseStack stack, int mouseX, int mouseY, float partialTicks) {
        super.render(stack, mouseX, mouseY, partialTicks);
        //Drawing Background
        super.renderBackground(stack);
        //Drawing Title
        drawCenteredString(stack, this.font, this.title, this.width / 2, 15, 16777215);
        //Draw or Render whatever you want, for example the player:
        if (minecraft.player != null)
            InventoryScreen.renderEntityInInventory(width - width/8, height/3 + 100, 75, 1.0f, 1.0f, this.minecraft.player);
    }

    @Override
    public void onClose() {
        super.onClose();
        System.out.println("Goodbye");
    }

    //Weather the screen should pause the game if shown
    @Override
    public boolean isPauseScreen() {
        return false;
    }
}

```

If I find small useful tricks I will update this.  
I hope I helped some of you a bit :)
