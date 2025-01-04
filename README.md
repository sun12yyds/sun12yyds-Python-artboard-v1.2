# sun12yyds Python artboard v1.2
# 在v1.1新增功能
增强后的画板程序：
```python
import tkinter as tk
from tkinter.colorchooser import askcolor
from tkinter import filedialog
from PIL import Image, ImageDraw

class PaintApp:
    def __init__(self, root):
        self.root = root
        self.root.title("精美画板程序")

        # 初始化画布大小
        self.canvas_width = 800
        self.canvas_height = 600

        # 创建一个画布
        self.canvas = tk.Canvas(self.root, bg="white", width=self.canvas_width, height=self.canvas_height)
        self.canvas.pack()

        # 创建工具栏
        self.toolbar = tk.Frame(self.root)
        self.toolbar.pack()

        # 创建颜色选择按钮
        self.color_button = tk.Button(self.toolbar, text="选择颜色", command=self.choose_color)
        self.color_button.pack(side=tk.LEFT)

        # 创建清除按钮
        self.clear_button = tk.Button(self.toolbar, text="清除画布", command=self.clear_canvas)
        self.clear_button.pack(side=tk.LEFT)

        # 创建保存按钮
        self.save_button = tk.Button(self.toolbar, text="保存", command=self.save_canvas)
        self.save_button.pack(side=tk.LEFT)

        # 创建画笔大小选择
        self.brush_size = tk.IntVar(value=2)
        self.brush_size_slider = tk.Scale(self.toolbar, from_=1, to=10, orient=tk.HORIZONTAL, label="画笔大小", variable=self.brush_size)
        self.brush_size_slider.pack(side=tk.LEFT)

        self.paint_color = "black"

        # 绑定鼠标事件
        self.canvas.bind("<B1-Motion>", self.paint)
        self.canvas.bind("<ButtonRelease-1>", self.reset)

        # 初始化画笔位置和画布图像
        self.old_x = None
        self.old_y = None
        self.image = Image.new("RGB", (self.canvas_width, self.canvas_height), "white")
        self.draw = ImageDraw.Draw(self.image)

    def choose_color(self):
        self.paint_color = askcolor(color=self.paint_color)[1]

    def clear_canvas(self):
        self.canvas.delete("all")
        self.image = Image.new("RGB", (self.canvas_width, self.canvas_height), "white")
        self.draw = ImageDraw.Draw(self.image)

    def paint(self, event):
        brush_size = self.brush_size.get()
        if self.old_x and self.old_y:
            self.canvas.create_line(self.old_x, self.old_y, event.x, event.y, fill=self.paint_color, width=brush_size)
            self.draw.line([self.old_x, self.old_y, event.x, event.y], fill=self.paint_color, width=brush_size)
        self.old_x = event.x
        self.old_y = event.y

    def reset(self, event):
        self.old_x = None
        self.old_y = None

    def save_canvas(self):
        file_path = filedialog.asksaveasfilename(defaultextension=".png", filetypes=[("PNG files", "*.png"), ("All files", "*.*")])
        if file_path:
            self.image.save(file_path)

if __name__ == "__main__":
    root = tk.Tk()
    app = PaintApp(root)
    root.mainloop()
```
