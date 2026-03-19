provider "aws" {
  region = "ap-southeast-1"
}

# EC2 Instance
resource "aws_instance" "my_ec2" {
  ami           = "ami-0df7a207adb9748c7"  # Amazon Linux 2023 (Singapore)
  instance_type = "t2.micro"

  subnet_id = "subnet-00e50e9aa2f7a62ac"   # Your subnet

  tags = {
    Name = "terraform-ec2"
  }
}

# EBS Volume (1 GB)
resource "aws_ebs_volume" "my_ebs" {
  availability_zone = aws_instance.my_ec2.availability_zone
  size              = 1

  tags = {
    Name = "terraform-ebs"
  }
}

# Attach EBS to EC2
resource "aws_volume_attachment" "ebs_attach" {
  device_name = "/dev/xvdf"
  volume_id   = aws_ebs_volume.my_ebs.id
  instance_id = aws_instance.my_ec2.id
}
