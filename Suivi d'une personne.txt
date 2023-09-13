list_Personnes = RmList()
variable_ecart_lacet = 0
def user_defined_Vitesse():
    global variable_ecart_lacet
    global list_Personnes
    list_Personnes=RmList(vision_ctrl.get_people_detection_info())
    if len(list_Personnes) < 2:
        chassis_ctrl.set_trans_speed(0)
    else:
        chassis_ctrl.set_trans_speed(-1.51 * list_Personnes[5] + 1.47)
def user_defined_Recherche():
    global variable_ecart_lacet
    global list_Personnes
    vision_ctrl.enable_detection(rm_define.vision_detection_people)
    robot_ctrl.set_mode(rm_define.robot_mode_chassis_follow)
    list_Personnes=RmList(vision_ctrl.get_people_detection_info())
    if len(list_Personnes) < 2:
        gimbal_ctrl.set_rotate_speed(30)
        gimbal_ctrl.rotate_with_degree(rm_define.gimbal_right,10)
    else:
        gimbal_ctrl.angle_ctrl(96 * (list_Personnes[2] - 0.5), 54 * (0.5 - list_Personnes[3]))
def user_defined_Deplacement():
    global variable_ecart_lacet
    global list_Personnes
    vision_ctrl.enable_detection(rm_define.vision_detection_people)
    robot_ctrl.set_mode(rm_define.robot_mode_chassis_follow)
    while True:
        list_Personnes=RmList(vision_ctrl.get_people_detection_info())
        if len(list_Personnes) < 2:
            chassis_ctrl.set_trans_speed(0)
        else:
            gimbal_ctrl.angle_ctrl(96 * (list_Personnes[2] - 0.5), 54 * (0.5 - list_Personnes[3]))
            if list_Personnes[5] < 0.76:
                user_defined_Vitesse()

                chassis_ctrl.move(0)
            else:
                chassis_ctrl.move_degree_with_speed(0.1,180)
        if len(list_Personnes) < 2:
            user_defined_Recherche()

def start():
    global variable_ecart_lacet
    global list_Personnes
    while True:
        user_defined_Recherche()

        user_defined_Deplacement()