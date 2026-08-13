from manim import *
import numpy as np

class MotorboatVelocityScene(Scene):
    def construct(self):
        # 1. Create Title and Equation Header
        title = Text("Motorboat Velocity Explanation", font_size=32).to_edge(UP)
        equation = MathTex("v(t) = 10e^{-t}", color=BLUE).next_to(title, DOWN, buff=0.3)
        self.play(Write(title), Write(equation))
        self.wait(1)

        # 2. Setup the coordinate grid axes
        axes = Axes(
            x_range=[0, 5, 1],
            y_range=[0, 11, 2],
            x_length=7,
            y_length=4,
            axis_config={"include_numbers": True, "color": GRAY},
        ).shift(DOWN * 0.5)

        # Labels for axes
        x_label = axes.get_x_axis_label(Text("Time (t in sec)", font_size=16), edge=RIGHT, direction=DOWN)
        y_label = axes.get_y_axis_label(Text("Velocity (v in m/s)", font_size=16), edge=UP, direction=LEFT)

        self.play(Create(axes), FadeIn(x_label), FadeIn(y_label))
        self.wait(1)

        # 3. Define the exponential curve v(t) = 10 * exp(-t)
        curve = axes.plot(
            lambda t: 10 * np.exp(-t),
            x_range=[0, 5],
            color=BLUE,
            stroke_width=4
        )

        # Animate tracing the graph lines smoothly
        self.play(Create(curve), run_time=3, rate_func=linear)
        self.wait(1)

        # 4. Target Evaluation at t = 2 seconds
        t_val = 2
        v_val = 10 * np.exp(-t_val) # approx 1.35

        # Create lines intersecting the point (2, 1.35)
        dot = Dot(color=RED).move_to(axes.c2p(t_val, v_val))
        h_line = axes.get_horizontal_line(axes.c2p(t_val, v_val), color=YELLOW, line_func=DashedLine)
        v_line = axes.get_vertical_line(axes.c2p(t_val, v_val), color=YELLOW, line_func=DashedLine)

        # Result display tags
        label_text = MathTex("t = 2\\text{ s} \\implies v \\approx 1.35\\text{ m/s}", font_size=24, color=RED)
        label_text.next_to(dot, UR, buff=0.2)

        # Play final highlight animations
        self.play(Create(v_line), Create(h_line))
        self.play(GrowFromCenter(dot))
        self.play(Write(label_text))
        self.wait(3)

