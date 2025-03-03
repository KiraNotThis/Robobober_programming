package org.firstinspires.ftc.teamcode.AUTO;

import com.qualcomm.hardware.rev.RevHubOrientationOnRobot;
import com.qualcomm.robotcore.eventloop.opmode.Autonomous;
import com.qualcomm.robotcore.eventloop.opmode.Disabled;
import com.qualcomm.robotcore.eventloop.opmode.LinearOpMode;
import com.qualcomm.robotcore.hardware.DcMotor;
import com.qualcomm.robotcore.hardware.IMU;
import com.qualcomm.robotcore.hardware.Servo;
import com.qualcomm.robotcore.hardware.TouchSensor;

import org.firstinspires.ftc.robotcore.external.navigation.AngleUnit;
import org.firstinspires.ftc.robotcore.external.navigation.YawPitchRollAngles;

@Autonomous(name="Methods70%", group="Robot")
public class Methods70 extends LinearOpMode {
    private DcMotor LeftFront = null;
    private DcMotor LeftBack = null;
    private DcMotor RightFront = null;
    private DcMotor RightBack = null;
    private DcMotor Vertical = null;
    private DcMotor Horizontal = null;

    private IMU imu = null;


    private Servo VerRotate;
    private Servo VerClaw;
    private Servo HorRotate;
    private Servo HorClaw;
    static final double FORWARD = 0.6;
    static final double ROTATE = 0.2;
    static final double WHEEL_DIAMETER = 10.4;
    static final double PULSES = 537.7;
    static final double PI = 3.1415;
    static final double PULSES_PER_CM = PULSES/(WHEEL_DIAMETER*PI);
    TouchSensor touchSensor;
    TouchSensor touchSensorHor;
    //TouchSensor touchSensorHor;











    @Override
    public void runOpMode() {
        touchSensor = hardwareMap.get(TouchSensor.class, "sensor_touch");//in configuration change the name
        touchSensorHor = hardwareMap.get(TouchSensor.class, "sensor_touch_hor");

        LeftFront = hardwareMap.get(DcMotor.class, "left_front");
        LeftBack = hardwareMap.get(DcMotor.class, "left_back");
        RightFront = hardwareMap.get(DcMotor.class, "right_front");
        RightBack = hardwareMap.get(DcMotor.class, "right_back");

        Vertical = hardwareMap.get(DcMotor.class, "Vertical");
        Horizontal = hardwareMap.get(DcMotor.class, "Horizontal");

        VerRotate = hardwareMap.get(Servo.class, "Vertical Rotate");
        VerRotate.setPosition(0.15);
        VerClaw = hardwareMap.get(Servo.class, "Vertical Claw");
        VerClaw.setPosition(0.5);

        HorRotate = hardwareMap.get(Servo.class, "Horizontal Rotate");
        HorRotate.setPosition(0.45);
        HorClaw = hardwareMap.get(Servo.class, "Horizontal Claw");
        HorClaw.setPosition(0.2);
        RevHubOrientationOnRobot.LogoFacingDirection logoDirection =
                RevHubOrientationOnRobot.LogoFacingDirection.RIGHT;

        RevHubOrientationOnRobot.UsbFacingDirection usbDirection =
                RevHubOrientationOnRobot.UsbFacingDirection.UP;

        RevHubOrientationOnRobot orientationOnRobot = new
                RevHubOrientationOnRobot(logoDirection, usbDirection);

        imu = hardwareMap.get(IMU.class, "imu");
        imu.initialize(new IMU.Parameters(orientationOnRobot));

        LeftFront.setDirection(DcMotor.Direction.FORWARD);
        LeftBack.setDirection(DcMotor.Direction.FORWARD);
        RightFront.setDirection(DcMotor.Direction.REVERSE);
        RightBack.setDirection(DcMotor.Direction.REVERSE);

        Vertical.setDirection(DcMotor.Direction.FORWARD);
        Horizontal.setDirection(DcMotor.Direction.FORWARD);

        LeftFront.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        LeftBack.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        RightFront.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        RightBack.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);

        Vertical.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        Horizontal.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);

        LeftFront.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
        LeftBack.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
        RightFront.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
        RightBack.setMode(DcMotor.RunMode.RUN_USING_ENCODER);

        Vertical.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
        Horizontal.setMode(DcMotor.RunMode.RUN_USING_ENCODER);

        imu.resetYaw();

        while (opModeInInit()) {

            telemetry.addData("Currently at:", "%4.0f", getHeading());
            telemetry.update();

        }

        waitForStart();

        driveStraight(1, 76.8);//Drive straight to the chamber
        driveStraight(-1, 6);//Drive back
        driveSide(1, 128.4);//Drive sideways for the sample
        driveSide(-1, 30);//Drive sideways
        driveStraight(-1, 70.8);//Drive backwards
        driveDiagonal(1, 96.7);//Drive diagonal to the chamber
        driveStraight(1, 13.2);//Drive straight
        driveDiagonal(-1, 98.9);//Drive diagonal to the observation
        driveStraight(-1, 11.6);//Drive backwards
        driveDiagonal(1, 98.9);//Drive diagonal to the chamber
        driveStraight(1, 11.6);//Drive straight
        driveDiagonal(-1, 106);//Drive diagonal to the observation
        driveSide(-1, 20);//Drive sideways




    }
    public void driveRotate(double rotateSpeed, double angle)
    {
        imu.resetYaw();

        while (opModeIsActive() && Math.abs(getHeading()) < angle) {

            telemetry.addData("Currently at:", "%4.0f", getHeading());
            telemetry.update();

            LeftFront.setPower(-rotateSpeed);
            LeftBack.setPower(-rotateSpeed);
            RightFront.setPower(rotateSpeed);
            RightBack.setPower(rotateSpeed);
        }
        LeftFront.setPower(0);
        LeftBack.setPower(0);
        RightFront.setPower(0);
        RightBack.setPower(0);
        sleep(300);
    }

    //______________________________*Straight_________________________________//

    public void driveStraight(double driveSpeed, double distance) {
        // Сбрасываем энкодеры перед началом движения
        LeftFront.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        LeftBack.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        RightFront.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        RightBack.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);

        // Включаем режим использования энкодеров
        LeftFront.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
        LeftBack.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
        RightFront.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
        RightBack.setMode(DcMotor.RunMode.RUN_USING_ENCODER);

        int targetTicks = (int) (PULSES_PER_CM * distance);

        // Начинаем замедляться на 70% пути
        double slowdownStart = targetTicks * 0.7;

        while (opModeIsActive() && Math.abs(LeftFront.getCurrentPosition()) < targetTicks) {
            int currentTicks = Math.abs(LeftFront.getCurrentPosition());
            double slowdownFactor = 1.0;

            // Плавное торможение, если робот проходит 70% расстояния
            if (currentTicks > slowdownStart) {
                slowdownFactor = Math.max(0.2, 1.0 - ((currentTicks - slowdownStart) / (targetTicks - slowdownStart)));
            }

            double adjustedSpeed = driveSpeed * slowdownFactor;

            telemetry.addData("Position", "%5d / %5d", currentTicks, targetTicks);
            telemetry.addData("Speed Factor", "%.2f", slowdownFactor);
            telemetry.update();

            LeftFront.setPower(adjustedSpeed);
            LeftBack.setPower(adjustedSpeed);
            RightFront.setPower(adjustedSpeed);
            RightBack.setPower(adjustedSpeed);
        }

        // Останавливаем моторы, но не ставим жесткий 0, чтобы сохранить накатывание
        LeftFront.setPower(0.05);
        LeftBack.setPower(0.05);
        RightFront.setPower(0.05);
        RightBack.setPower(0.05);

        // Небольшая задержка для завершения движения по инерции
        sleep(200);

        // Полностью отключаем моторы
        LeftFront.setPower(0);
        LeftBack.setPower(0);
        RightFront.setPower(0);
        RightBack.setPower(0);
    }
    //______________________________Straight*_________________________________//


    //______________________________*Side_____________________________________//

    public void driveSide(double driveSpeed, double distance) {
        // Сбрасываем энкодеры перед началом движения
        LeftFront.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        LeftBack.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        RightFront.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        RightBack.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);

        // Включаем режим использования энкодеров
        LeftFront.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
        LeftBack.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
        RightFront.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
        RightBack.setMode(DcMotor.RunMode.RUN_USING_ENCODER);

        int targetTicks = (int) (PULSES_PER_CM * distance);

        // Начинаем замедляться после прохождения 70% пути
        double slowdownStart = targetTicks * 0.7;

        while (opModeIsActive() && Math.abs(LeftFront.getCurrentPosition()) < targetTicks) {
            int currentTicks = Math.abs(LeftFront.getCurrentPosition());
            double slowdownFactor = 1.0;

            // Плавное торможение, если робот проходит 70% расстояния
            if (currentTicks > slowdownStart) {
                slowdownFactor = Math.max(0.2, 1.0 - ((currentTicks - slowdownStart) / (targetTicks - slowdownStart)));
            }

            double adjustedSpeed = driveSpeed * slowdownFactor;

            telemetry.addData("Position", "%5d / %5d", currentTicks, targetTicks);
            telemetry.addData("Speed Factor", "%.2f", slowdownFactor);
            telemetry.update();

            // Боковое движение с учетом замедления
            LeftFront.setPower(adjustedSpeed);
            LeftBack.setPower(-adjustedSpeed);
            RightFront.setPower(-adjustedSpeed);
            RightBack.setPower(adjustedSpeed);
        }

        // Оставляем небольшой накат перед полной остановкой
        LeftFront.setPower(0.05);
        LeftBack.setPower(-0.05);
        RightFront.setPower(-0.05);
        RightBack.setPower(0.05);

        // Короткая пауза для естественного торможения
        sleep(200);

        // Полностью отключаем моторы
        LeftFront.setPower(0);
        LeftBack.setPower(0);
        RightFront.setPower(0);
        RightBack.setPower(0);
    }
    //______________________________Side*_____________________________________//


    //___________________________*Diagonal____________________________________//

    public void driveDiagonal(double driveSpeed, double distance) {
        // Сбрасываем энкодеры перед началом движения
        LeftFront.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        LeftBack.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        RightFront.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        RightBack.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);

        // Включаем режим использования энкодеров
        LeftFront.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
        LeftBack.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
        RightFront.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
        RightBack.setMode(DcMotor.RunMode.RUN_USING_ENCODER);

        int targetTicks = (int) (PULSES_PER_CM * distance);

        // Начинаем замедляться после прохождения 70% пути
        double slowdownStart = targetTicks * 0.7;

        while (opModeIsActive() && Math.abs(RightFront.getCurrentPosition()) < targetTicks) {
            int currentTicks = Math.abs(RightFront.getCurrentPosition());
            double slowdownFactor = 1.0;

            // Плавное торможение, если робот проходит 70% расстояния
            if (currentTicks > slowdownStart) {
                slowdownFactor = Math.max(0.2, 1.0 - ((currentTicks - slowdownStart) / (targetTicks - slowdownStart)));
            }

            double adjustedSpeed = driveSpeed * slowdownFactor;

            telemetry.addData("Position", "%5d / %5d", currentTicks, targetTicks);
            telemetry.addData("Speed Factor", "%.2f", slowdownFactor);
            telemetry.update();

            // Диагональное движение с учетом замедления
            LeftFront.setPower(0);
            LeftBack.setPower(-adjustedSpeed);
            RightFront.setPower(-adjustedSpeed);
            RightBack.setPower(0);
        }

        // Оставляем небольшой накат перед полной остановкой
        LeftBack.setPower(-0.05);
        RightFront.setPower(-0.05);

        // Короткая пауза для естественного торможения
        sleep(200);

        // Полностью отключаем моторы
        LeftFront.setPower(0);
        LeftBack.setPower(0);
        RightFront.setPower(0);
        RightBack.setPower(0);
    }
    //___________________________Diagonal*____________________________________//

    public void VerSliderPosition(double position, double power){
        Vertical.setTargetPosition((int) position);
        Vertical.setMode(DcMotor.RunMode.RUN_TO_POSITION);
        Vertical.setPower(power);
    }

    public void VerSliderZero(double power){

        while (opModeIsActive() && !touchSensor.isPressed()) {
            Vertical.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
            Vertical.setPower(power);  // Keep moving down
        }

        // Stop the vertical motor once the sensor is pressed
        Vertical.setPower(0);
    }

    public void HorSliderPosition(double position, double power){
        Horizontal.setTargetPosition((int) position);
        Horizontal.setMode(DcMotor.RunMode.RUN_TO_POSITION);
        Horizontal.setPower(power);
    }

    public void HorSliderZero(double power){

        while (opModeIsActive() && !touchSensorHor.isPressed()) {
            Horizontal.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
            Horizontal.setPower(power);  // Keep moving down
        }

        // Stop the vertical motor once the sensor is pressed
        Horizontal.setPower(0);
    }

    public double getHeading()
    {
        YawPitchRollAngles orientation = imu.getRobotYawPitchRollAngles();
        return orientation.getYaw(AngleUnit.DEGREES);
    }
}
