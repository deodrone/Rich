use gpui::*;

actions!(cashpro, [Quit]);

fn main() {
    App::new().run(|cx: &mut AppContext| {
        // Register application actions
        cx.activate(true);
        cx.on_action(|_: &Quit, cx| cx.quit());
        
        // Set global styles
        cx.set_global(TextStyle {
            font_family: "Inter".into(),
            font_size: px(14.),
            color: Color::black(),
            ..Default::default()
        });
        
        // Create main window
        let options = WindowOptions {
            bounds: WindowBounds::Fixed(Bounds {
                size: size(px(800.), px(600.)).into(),
                origin: Default::default(),
            }),
            title: "Cash Pro".into(),
            ..Default::default()
        };
        
        cx.open_window(options, |cx| {
            // Main application view
            View::build(|cx| {
                // Tabbed interface
                TabBar::new(
                    cx,
                    |tab_bar| {
                        // Dashboard Tab
                        Tab::new(
                            tab_bar,
                            "Dashboard",
                            |cx| {
                                Element::new(cx, |cx| {
                                    VStack::new(cx, |cx| {
                                        // Balance Card
                                        Card::new(cx, |cx| {
                                            Label::new(cx, "Total Balance")
                                                .font_size(px(18.));
                                            Label::new(cx, "$5,000.00")
                                                .font_size(px(32.));
                                        })
                                        .width(Stretch(1.0))
                                        .height(Auto));
                                        
                                        // Quick Actions
                                        HStack::new(cx, |cx| {
                                            Button::new(
                                                cx,
                                                |cx| cx.emit(AppEvent::SendMoney),
                                                |cx| Label::new(cx, "Send Money")
                                            );
                                            
                                            Button::new(
                                                cx,
                                                |cx| cx.emit(AppEvent::RequestMoney),
                                                |cx| Label::new(cx, "Request Money")
                                            );
                                            
                                            Button::new(
                                                cx,
                                                |cx| cx.emit(AppEvent::Invest),
                                                |cx| Label::new(cx, "Invest")
                                            );
                                        })
                                        .child_space(Stretch(1.0));
                                        
                                        // Recent Transactions
                                        List::new(cx, |cx| {
                                            ListItem::new(cx, |cx| {
                                                Label::new(cx, "Received from John Doe");
                                                Label::new(cx, "+$100.00");
                                            });
                                            
                                            ListItem::new(cx, |cx| {
                                                Label::new(cx, "Sent to Jane Smith");
                                                Label::new(cx, "-$50.00");
                                            });
                                        });
                                    })
                                    .child_space(Stretch(1.0))
                                })
                            },
                        );
                        
                        // Payments Tab
                        Tab::new(
                            tab_bar,
                            "Payments",
                            |cx| {
                                // Payments UI would go here
                            },
                        );
                        
                        // Investing Tab
                        Tab::new(
                            tab_bar,
                            "Investing",
                            |cx| {
                                // Investing UI would go here
                            },
                        );
                    },
                )
            })
        });
    });
}

enum AppEvent {
    SendMoney,
    RequestMoney,
    Invest,
}
